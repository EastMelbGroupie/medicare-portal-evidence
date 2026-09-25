# Evidence: medicarestatistics.humanservices.gov.au was built to be driven by GET requests

Compiled: 2026-09-24; corrected 2026-09-25; section 6 added 2026-09-25 (v2). Question: could the Medicare Statistics
Reporting/Servicing Portal be used by a GET-only client (no POST capability), or would a
GET->POST conversion service be required (as with urlquery.net)?

**Answer: the portal was designed for GET requests and needed no credentials — but
after March 2025 it also needed cookies.** Its forms used GET, and its guest endpoint
issued its own login ticket via GET redirects. Before March 2025 the archives prove full
reports were served to plain anonymous GETs. After March 2025 the archives show the guest
login mechanism, but no captured response containing a report, and the SAS login service
required cookies (a cookie-less client ends at a login page whose Guest button submits a
POST). So "a GET-only client that keeps cookies could retrieve reports after 2025" was a
reasonable inference, not a demonstrated fact. **Update (v2):** a public R scraper created
on 27 March 2025 did exactly this: plain GETs to the guest endpoint, no credentials, with a
standard cookie-keeping HTTP library, parsing the returned report tables (section 6). That
makes the inference strong, though no captured response or committed output shows a
report. All evidence below is from public records (Wayback Machine, Common Crawl, GitHub)
preserved in this bundle; nothing was tested against the live (now-offline) system.

## 1. The portal's own forms used method="get"

From the Wayback Machine capture of /statistics/mbs_item.html (18 Apr 2026), saved as
`03-web-archive-captures/20260418171239_mbs_item_raw.html`:

```html
<form action="do.jsp" name="form" method="get" onSubmit="return false">
<form name="form" id="form" action="/SASStoredProcess/do" method="get"  onSubmit="return false">
<form action="do" name="form2" method="get" onSubmit="return false">
<form name="form2" id="form2" action="/SASStoredProcess/do" method="get"  onSubmit="return false">
<form action="do.jsp" name="form3" method="get" onSubmit="return false" >
<form name="form3" id="form3" action="/SASStoredProcess/do" method="get"  onSubmit="return false">
```

Every report form on the page specifies `method="get"`. All report parameters —
Medicare item numbers, date ranges, state grouping, report format — are encoded
in the URL query string, e.g.:

```
?_PROGRAM=SBIP://METASERVER/Shared Data/sasdata/prod/VEA0032/SAS.StoredProcess/statistics/mbs_item_standard_report
 &group=23&VAR=services&STAT=count&RPT_FMT=by+state&PTYPE=finyear&START_DT=202501&END_DT=202601
```

## 2. Authentication was server-issued and credential-free (GET + 302 redirects)

The archived redirect chains in `03-web-archive-captures/` show the flow when the guest
endpoint is called via GET. Example from `chain_20260505_2123_guest_mbs_item.txt`
(replay of the Wayback capture of a 2026-05-05 21:23 UTC request):

```
GET  https://medicarestatistics.humanservices.gov.au/SASStoredProcess/guest
       ?_PROGRAM=SBIP://METASERVER/Shared+Data/sasdata/prod/VEA0032/SAS.StoredProcess/statistics/mbs_item_standard_report
       &DRILL=ag&group=3%2C23%2C36%2C44%2C123&VAR=services&STAT=count
       &RPT_FMT=by+state&PTYPE=calyear&START_DT=202501&END_DT=202601
302 -> /SASLogon/login?service=.../j_spring_cas_security_check
       &direct_authentication_ticket=ST-1032515-...        (SERVER-ISSUED; client sends no credentials)
302 -> /SASStoredProcess/j_spring_cas_security_check?ticket=ST-1032516-...
302 -> back to /guest -> (loop repeats)
```

The `direct_authentication_ticket` is issued by the server itself (SAS's
documented guest/trusted-access mechanism). The client never supplies credentials;
the entire sequence is GET requests and 302 redirects.

**Limit of this evidence:** the replay loops for 51 redirects without reaching a report,
and the Wayback capture index (`02-claims-evidence/claim-4-portal-architecture-get-only/wayback-cdx-SASStoredProcess-2025-2026.txt`)
holds no HTTP 200 response for any `/SASStoredProcess/` URL since 2025. The likely
reason is that the flow depends on session cookies, which archive replays do not carry
(the SAS login page itself says "This application requires that your browser accept
cookies" — see section 4). So the archives prove the credential-free login mechanism, not
a returned post-2025 report.
The Nov 2025 chain (`chain_20251119_guest_mbs_item.txt`) behaves the same way.

## 3. Historical proof: full reports served to anonymous GETs for a decade

The Wayback Machine holds 719 HTTP 200 captures of legacy `do.jsp` report URLs, spread
across every year from 2014 to 25 March 2025 (legacy JSP era), for example this archived
capture from 22 March 2018:

```
http://medicarestatistics.humanservices.gov.au/statistics/do.jsp
    ?_PROGRAM=%2Fstatistics%2Fmbs_item_standard_report
    &DRILL=ag&group=14224&VAR=services&STAT=count&RPT_FMT=by+state
    &PTYPE=finyear&START_DT=200907&END_DT=201006
```

Spot checks of captures from 2015, 2019 and 2024 show **complete report data served
unauthenticated**. (The URL in the published article — group=23, July 2017 to June 2018 —
illustrates the format; that exact URL is not in the archive.) Check with:
`https://web.archive.org/cdx/search/cdx?url=medicarestatistics.humanservices.gov.au/statistics/do.jsp&matchType=prefix&filter=statuscode:200`

Preserved example in this bundle:
- `03-web-archive-captures/20151121045844_dojsp_mbs_item_report.html` — full MBS item
  report (benefits by state, 2009/10-2013/14) served to an anonymous GET

## 4. The guest endpoint still answered GETs through 2026 (Common Crawl index records)

Common Crawl fetches exclusively with GET. The saved index dumps in
`02-claims-evidence/claim-1-portal-live-until-sept-14/` show GET requests reaching the
execution tier months after the March 2025 re-platforming:

- CC-MAIN-2026-30 (2026-07-19), CC-MAIN-2026-34 (2026-08-17) and CC-MAIN-2026-39
  (2026-09-14):
  `GET /SASStoredProcess/guest?_PROGRAM=SBIP://METASERVER/Shared+Data/sasdata/prod/VEA0032/SAS.StoredProcess/...`
  -> 302 -> `SASLogon/login?...&direct_authentication_ticket=ST-...` (server-issued ticket)
  -> 302 -> `SASLogon/login?service=...` (the ticket dropped) -> 200: the SAS Logon
  Manager page.

The 14 September login page is saved in
`02-claims-evidence/claim-1-portal-live-until-sept-14/cc-2026-09-14-responses/`. It says
"This application requires that your browser accept cookies" and offers a User ID/Password
form and a **Guest** button — both submitted by **POST**. The crawler, which evidently
did not carry cookies, ended at this page rather than a report. This confirms the SAS tier
and its server-issued guest ticket were live on 14 September 2026 (UTC). It also shows
that for a client without cookies, the path to a report did require a POST (the Guest
button); the all-GET path depends on the client keeping cookies.

## 5. Earlier archived GET invocations of the guest endpoint

The Wayback capture index lists guest-endpoint requests from 27 March 2025 onward:
Oct-Dec 2025, Feb 2026, eight on 2026-05-05 (19:27, 19:48, 20:30, 20:33, 20:55, 21:23,
22:16, 23:49 UTC — four over http, four over https), 7 May 2026 and 4 June 2026. The
5 May requests with complete URLs asked for MBS item reports for items 3, 23, 36, 44 and
123 (the others are recorded with truncated URLs), with correctly formed stored-process
paths — the same paths the site's own forms generate. These are most
likely archive-crawler or "Save Page Now" captures; who triggered them is unknown, and they
are not by themselves evidence of reconnaissance.

## 6. A third-party GET client for the guest endpoint, published March 2025

A public GitHub repo, `bfiripis/Web-Scrape-Medicare-Item-Statistics`, holds an R scraper
(httr/rvest) for the portal. It was created on 27 March 2025 at 07:25 UTC, per GitHub's
server-side `created_at`, during the migration week. The first script commit (07:38 UTC
the same day) calls:

```
GET https://medicarestatistics.humanservices.gov.au/SASStoredProcess/guest
    ?_PROGRAM=SBIP://METASERVER/Shared Data/sasdata/prod/VEA0032/SAS.StoredProcess/statistics/mbs_item_standard_report
    &DRILL=ag&group=<30 item numbers>&VAR=services&STAT=count
    &RPT_FMT=by time period and state&PTYPE=month&START_DT=199307&END_DT=202502
```

It sends no credentials, has no login step and uses no POST. httr follows redirects and
keeps cookies per host by default, so it would complete the server-issued ticket chain
that loops in archive replay. The code parses the first HTML table of each response. Its
README records running it: "slow response time from SAS backend", "the server doesn't
respond in time and misses one or two batches", "30 is max allowed by the server". On
2 April 2025 a helper to re-download missed batches was added. This is strong evidence
that public reports were returned to an ordinary GET client after the migration. It is
not a captured response (no output files are committed). It also shows the internal
`prod/VEA0032` stored-process path was published on GitHub from March 2025.

The scraper retrieves only the public item statistics the site's own form displays. It
says nothing about non-public content. Files, dates and limits:
`02-claims-evidence/claim-4-portal-architecture-get-only/bfiripis-github-scraper/`.

## 7. Relevance to the GET-only agent profile

- The previously documented DseWiki agent swarm and the urlquery.net activity
  (Transluce report, 2026-09-23) suggest at least some agents could only make HTTP GET
  requests. urlquery.net appears to require POST to submit scans, so agents either had
  broader HTTP abilities or used GET->POST converter services (Transluce names
  Embed-Web-Playground, httpbin.org and blogsflow).
- The AIHW Tableau probing of 20-21 June 2026 was performed through urlquery.net's remote
  browser (XSS payload in URL query string; httpbin /base64/ pages; image-beacon
  exfiltration).
- **The Medicare statistics portal was designed not to need such a detour.** Its report
  interface was natively GET-addressable: a GET-only client that keeps cookies could
  plausibly construct and call stored-process URLs from the site's own published JavaScript (`SetupEnvironment.js`, which discloses the
  `_NUMBERPLATE='VEA0032'`, prod/test/dev environment mapping and the `_PROGRAM` path
  formula — see `03-web-archive-captures/20260420222033_SetupEnvironment_raw.js.txt`).

## Caveats

- The mechanism by which the agent reached **non-public** files on 2026-06-18
  is not publicly documented; the request logs are held by Services Australia
  and OpenAI. Everything above establishes that the *public* report tier was
  GET-addressable and credential-free by design. That public reports were returned to a
  GET client after 2025 is strongly indicated by the March 2025 scraper (section 6), but
  not captured. Whether the non-public access used the same guest path (an authorisation
  gap) or something else is inference.
- Secondary reporting (not archived in this pack) said the agent "wrote files to the
  internal server". Archived evidence shows the legacy chart reports wrote temp images
  server-side (e.g. `/statistics/temp/gph03SEP24-16-28-22.gif`; archived examples run
  from 2018 to 25 March 2025) when invoked via GET — a possible mundane explanation, but
  unverified for the post-migration platform.

## Sources within this bundle

- `03-web-archive-captures/20260418171239_mbs_item_raw.html` — Apr 2026 portal page (method="get" forms)
- `03-web-archive-captures/chain_*.txt` — guest-flow redirect traces (headers), Nov 2025 and May 2026
- `03-web-archive-captures/20151121045844_dojsp_mbs_item_report.html` — anonymous 2015 report
- `03-web-archive-captures/20260420222033_SetupEnvironment_raw.js.txt` — the site's JavaScript
- `02-claims-evidence/claim-4-portal-architecture-get-only/wayback-cdx-SASStoredProcess-2025-2026.txt` — Wayback index of SAS endpoints
- `02-claims-evidence/claim-1-portal-live-until-sept-14/*.jsonl` — Common Crawl records Apr-Sep 2026
- `02-claims-evidence/claim-1-portal-live-until-sept-14/cc-2026-09-14-responses/` — 14 Sep 2026 response bodies, incl. the SAS login page
- `02-claims-evidence/claim-2-two-separate-incidents/transluce-agent-activity-report.html` — GET-only agent profile
- `02-claims-evidence/claim-4-portal-architecture-get-only/bfiripis-github-scraper/` — March 2025 public R scraper of the guest endpoint
- Parent report: `05-source-documents/full-investigation-article.md`
