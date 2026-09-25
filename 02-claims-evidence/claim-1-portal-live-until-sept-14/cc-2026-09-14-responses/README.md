# Common Crawl response bodies, 14 September 2026 (UTC)

Retrieved 25 September 2026 from Common Crawl's public WARC files, using the `filename`,
`offset` and `length` fields in `../CC-MAIN-2026-39-medicarestatistics.jsonl`, e.g.:

    curl -r <offset>-<offset+length-1> https://data.commoncrawl.org/<filename> | gunzip

Times are UTC; add 10 hours for Canberra time (AEST).

| File | Captured (UTC) | Canberra time | What it is |
|---|---|---|---|
| `20260914153142_homepage.html` | 14 Sep 15:31 | 15 Sep 01:31 | Homepage: a meta-refresh redirect to `/statistics/mbs_item.jsp` |
| `20260914142334_mbs_item.jsp.html` | 14 Sep 14:23 | 15 Sep 00:23 | Redirect stub to `/statistics/mbs_item.html` (identical in every 2026 capture) |
| `20260914140907_SASLogon_login.html` | 14 Sep 14:09 | 15 Sep 00:09 | SAS Logon Manager 9.4 page, freshly generated (login ticket `LT-9913-…`): "This application requires that your browser accept cookies"; User ID/Password form and a **Guest** button, both submitted by POST |

What these show: the web server and the SAS application were live and generating fresh
login tickets. They do not show the report form page itself (`mbs_item.html`), which was
not in this crawl; its last archived capture is 11 August 2026 (Wayback Machine).
