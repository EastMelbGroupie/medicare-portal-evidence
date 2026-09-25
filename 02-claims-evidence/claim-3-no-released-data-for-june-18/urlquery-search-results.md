# urlquery.net search results

Searched: 2026-09-24, via rendered web interface at https://urlquery.net/search
(search is JS/HTMX; results verified in a real browser session)

Search URL pattern: https://urlquery.net/search?q=<query>&type=reports

## 1. http.url.addr:*SASStoredProcess*
Result: **"No reports found"** (0 reports)

## 2. http.url.addr:*medicarestatistics*
Result: **"No reports found"** (0 reports)

**Update (re-run 25 September 2026):** now 1 report — a scan of
medicarestatistics.humanservices.gov.au (203.13.0.23) dated 2026-09-24 15:37, after the
PM's announcement and after the searches above. `SASStoredProcess` still returns 0, and
the only .gov.au result for `humanservices` (now 42 hits) is the same 24 September scan.

## 3. http.url.addr:*medicare* — 23 results
None are Australian government sites. Hosts/URLs captured (truncated display URLs):
- links-2.govdelivery.com/CL0/https:%2F%2Fwww.franklincountyohio.gov%2Ffiles%2Fassets%2Fpublic%2Fv%2F4%2Fbenefits%2Fdocuments%2Flegal-documents%2F2026-medicare-d-notice.pdf/... (US county Medicare notice PDF)
- www.noridianmedicareportal.com/web/nmp/home... (US Medicare portal)
- click.members.bcbsm.com/?qs=...
- zoommedicaremeetings.com/
- discoverhealthplan.com
- medicareremedies.com
- www.jblindia.in
- selectmypolicy.com/
- opgguides.com/food-stamps-guide/?... (US benefits lead-gen)
- www.foundit.com.ph/job/staff-nurse-fresenius-medical-care-philippines-...
- medicarebenefitsinc.com/
- marketingwithsam.co/campaigns/.../track-url/...
- seattledermatology.com/
- aarpmaenroll.com
- urldefense.com/v3/__https://www.noridianmedicareportal.com/...__
- www.gettyimages.com/search/2/image?family=editorial&phrase=australia%20%22keating%22%201984&sort=oldest
- bendigoaircompressors.bondassist.com.au/ (only .com.au hit; not government, not Medicare)
- bendigoaircompressors.bondassist.com.au
- medicaresrx.com
- michalkostic.com
- bizmarketevolution.com/
- sapphirexchange.com/
- top-medicare.com/
Conclusion: all US Medicare or unrelated/spam. Zero .gov.au.

## 4. http.url.addr:*servicesaustralia* — 17 results
No scans of the real servicesaustralia.gov.au. Hits were:
- Phishing kits: govinbox-servicesaustralia.com/myGovLogin.php;
  secure.mygov.au.lasexecutione0s1.top/mygov-login; secure.mygov.au.lasexecutione0s2.top/mygov-login;
  newbnst.xyz/sakazulu/; youdontireke.xyz/documentss/
- Outlook safelinks containing @servicesaustralia.gov.au email addresses (mail-flow URLs):
  aus01.safelinks.protection.outlook.com/?url=https://www.bbsindiapvtltd.com/...medicare.education@servicesaustralia.gov.au...
  aus01.safelinks.protection.outlook.com/?url=https://panelist.com/Factor/crrsmcs&...Amanda.Baker@servicesaustralia.gov.au...
- Coincidental matches: dns-speed.tail-f.de; apexfundservicesaustraliasivptyltd.vu (multiple);
  www.securityservicesaustralia.com/; proudasservicesaustralia.au/;
  www.smartpostlandingservicesaustralia.com.au/; www.airservicesaustralia.com/publications/aip-australia
Conclusion: zero genuine servicesaustralia.gov.au site scans.

## 5. http.url.addr:*humanservices* — 24 results
No hits on humanservices.gov.au (Australian). Hits were:
- US HHS: ams.hhs.gov/oam/server/obrareq.cgi?... (several); ams.hhs.gov
- US county/org human services: baycovehumanservices.org/;
  humanserviceshungary.hu/xmlrpc.php/; rmhumanservices.login-us.mimecast.com;
  baycovehumanservices.org; jevshumanservices.org; mces.org; pioneerhumanservices.org;
  vwu.edu; encryptmailcswshumanservicesdhspagov.vonza.com/; hhsof.servicenowservices.com/;
  humanservices.directsmailsolution.com/qrc-ac/... (2x)
- Mail-tracking URLs containing humanservices@ email addresses: dm-us/dm-cn/dm-sg.aliyuncs.com/trace/v1/report?...humanservices@crawfordcountywi.org... (several)
- Misc: eadn-wc03-8732992.nxedge.io/; krcrtv.com/; turkeypurge.com;
  luckys-as8.pages.dev/government?...; mcdonaldschaefersupport.com/
Conclusion: zero medicarestatistics.humanservices.gov.au traces.

## 6. Control: http.url.addr:*aihw* — 24+ results (search functional)
Returned 24 report links on first page (search index includes all URLs fetched in a scan).

## 7. Control: http.url.addr:*medicare* — 23 results (see #3; search functional)

Notes:
- urlquery.net HTMX API endpoint https://urlquery.net/api/htmx/search/ returns
  HTTP 204 for direct (non-JS) clients; results must be rendered in a browser.
- Searches for SASStoredProcess/medicarestatistics re-verified twice (12-15s waits
  after render) — both consistently "No reports found".
- Transluce dataset (https://transluce.org/data/urlquery-agent-activity-2026-09-23.zip)
  also contains zero matches for: SASStoredProcess, medicarestatistics, medicare,
  storedprocess (checked all 16 files, case-insensitive). This is not evidence either
  way: the dataset is a catalog of report IDs/metadata, not the underlying scanned URLs,
  and it was built by enumerating ~40 specific domains that did not include
  medicarestatistics.humanservices.gov.au (see its search-coverage.json).
