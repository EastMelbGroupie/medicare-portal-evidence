# Claim 4 — The portal's reporting engine was designed for plain GET requests with a password-free guest login; no exploit is indicated

**The claims (from the article and tweets), with their evidential status:**
1. **Proven.** The portal's report forms used `method="get"` — all report parameters
   travelled in URLs.
2. **Proven (mechanism) / not proven (outcome).** The execution tier
   (`/SASStoredProcess/guest?...`) performed a server-driven login: the server issued its own
   `direct_authentication_ticket`, with no credentials sent by the client. What no archive
   shows is that flow ending in a returned report after the March 2025 migration — every
   archived guest call ends in a redirect loop (Wayback) or at the SASLogon login page
   (Common Crawl). That login page (saved in
   `../claim-1-portal-live-until-sept-14/cc-2026-09-14-responses/`) states "This
   application requires that your browser accept cookies", and its **Guest** button submits
   the form by POST. So the server-issued guest login was a GET-only flow only for a client
   that keeps cookies; a client without cookies ends at a page that needs a POST. The
   article's "back to /guest → report runs" is inference.
   **Update (v2): strong circumstantial support.** A public R scraper, created on GitHub on
   27 March 2025, calls `/SASStoredProcess/guest` with plain GETs and no credentials,
   using a standard HTTP library that keeps cookies. It parses the returned report table,
   and its notes and later fixes record running it against the server. See
   `bfiripis-github-scraper/`. The outcome is still not a captured response (no output is
   committed), but it is no longer only reconstruction.
3. **Proven.** The site's own JavaScript (`SetupEnvironment.js`) published the internal
   path formula (`VEA0032`, prod/test/dev mapping). The file first appears in the archive on
   27 March 2025, with the new platform. Before the migration, full reports were already
   served to anyone, unauthenticated, at plain `do.jsp` URLs.
4. **Plausible, unverified.** The reported "wrote files to the internal server" detail
   (secondary reporting, not archived in this pack) has a mundane candidate explanation:
   chart reports wrote image files to `/statistics/temp/` on the server. The archived temp
   files (2018 to 25 March 2025) all date from the legacy JSP platform; whether the
   post-migration platform does the same is not shown.

## Evidence in this folder (archived copies; originals in `../03-web-archive-captures/`)

- **`20260418171239_mbs_item_raw.html`** — Wayback Machine capture of the portal's main
  report page (`/statistics/mbs_item.html`), 18 April 2026. Contains six forms, all
  `method="get"`, e.g.:
  `<form name="form" id="form" action="/SASStoredProcess/do" method="get"  onSubmit="return false">`
  Verify at: https://web.archive.org/web/20260418171239id_/https://medicarestatistics.humanservices.gov.au/statistics/mbs_item.html
- **`20260420222033_SetupEnvironment_raw.js.txt`** — the site's own JavaScript, archived
  20 Apr 2026 (content unchanged in every capture since 30 April 2025). Contains verbatim:
  `var _NUMBERPLATE='VEA0032';`, the 01/02/03 → prod/test/dev mapping, and the
  `_PROGRAM='/Shared Data/sasdata/'+ENV_SYSTEM+'/'+_NUMBERPLATE+...` path formula.
  Verify at: https://web.archive.org/web/20260420222033id_/https://medicarestatistics.humanservices.gov.au/VEA0032/SAS.Web/MCACommon/SetupEnvironment.js
- **`chain_20260505_2123_guest_mbs_item.txt`** — HTTP headers from replaying the Wayback
  capture of a guest call made 5 May 2026 at 21:23 UTC:
  `GET /SASStoredProcess/guest?_PROGRAM=SBIP://METASERVER/Shared+Data/sasdata/prod/VEA0032/...`
  → `SASLogon/login?...&direct_authentication_ticket=ST-1032515-...` (server-issued)
  → `j_spring_cas_security_check?ticket=ST-1032516-...` → back to `/guest` → and round
  again. The replay loops through 51 redirects without reaching a report (HTTP 200); the
  archive did not capture a report response.
- **`wayback-cdx-SASStoredProcess-2025-2026.txt`** — the Wayback capture index for every
  `/SASStoredProcess/` URL since 2025. None of the 57 entries has status 200. Guest-endpoint
  calls were captured from 27 March 2025 onward: Oct-Dec 2025, Feb 2026, eight on 5 May
  2026 (19:27-23:49 UTC, i.e. 05:27-09:49 on 6 May, Canberra time), 7 May 2026 and
  4 June 2026. They are most likely archive-crawler
  captures of the site's own form target; who triggered them is unknown, and they are not
  evidence of reconnaissance.
- **`bfiripis-github-scraper/`** (added v2) — a public R scraper on GitHub (repo created
  27 March 2025, 07:25 UTC) that retrieves MBS/PBS item reports with plain GETs to
  `/SASStoredProcess/guest`, using the internal `SBIP://…/prod/VEA0032/…` path and no
  credentials. It includes the original commit, the current scripts, the git history, the
  GitHub API record and the @foilmanhacks tweet that surfaced it. See its `evidence.md`.

## Full write-up

See **`../04-get-only-access/GET-only-access-evidence.md`** — the evidence document covering
the method="get" forms, the credential-free guest login, a decade of anonymous
GET-served reports before 2025 (a 2015 example is in `../03-web-archive-captures/`), Common
Crawl captures through 2026, and caveats.

## Caveat

The article's reconstruction of the incident mechanism is inference from archived public
records. What is proven: the portal served full reports to anonymous GET requests from
2014 to early 2025; after March 2025 its forms still used GET, its guest endpoint issued
its own login ticket without credentials, the SAS tier was still answering on 14 September
2026 (UTC), and the site published its own internal addressing. What is strongly
indicated but not captured: that the post-2025 guest flow returned public reports to an
ordinary cookie-keeping GET client. A third-party scraper published on 27 March 2025 was
built and maintained to do exactly that (`bfiripis-github-scraper/`), but no archived
response or committed output shows a report. What is not publicly known: whether
non-public content was reachable the same way, and exactly which requests the agent made
on 18 June.
