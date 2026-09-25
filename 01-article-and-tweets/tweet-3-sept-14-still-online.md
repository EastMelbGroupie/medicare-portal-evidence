# Tweet 3 — Portal still online on 14 September (UTC), after disclosure

**Source:** https://x.com/EastMelbGroupie/status/2103107324558848331
**Author:** East Melb Groupie (@EastMelbGroupie) · 24 Sep 2026, 11:00 PM
**Text:**

> Lol. Government was informed on September 10 and the site was still up on September 14.
> No one working over the weekend?

**Screenshot:** `screenshots/HS-8xEsbQAAfYDV.png`

## Transcript of attached screenshot (OCR, typos corrected)

> One factual update the article needs: line 107 says the site stayed up "at least eight
> weeks after the breach" (citing June 26 and Aug 11 captures). My Common Crawl data extends
> that substantially: CC-MAIN-2026-39 shows the portal still serving HTTP 200 on September
> 14, 2026 — nearly 13 weeks after the breach, and four days after OpenAI's Sept 10
> disclosure email, one day before Services Australia notified ASD. So the take-down
> happened between Sept 14-24, only after verification/escalation. The detection-failure
> point gets stronger, not weaker, with this.

## Claims made (see `../02-claims-evidence/claim-1-portal-live-until-sept-14/`)

1. OpenAI informed the government on 10 September 2026 → ABC live blog (disclosure timeline)
2. The portal was still serving normally on 14 September 2026 → **Common Crawl index
   records (CC-MAIN-2026-39), captured 2026-09-14: HTTP 200 on the homepage and
   /statistics/mbs_item.jsp; SASLogon and SASStoredProcess/guest endpoints still responding**
3. Services Australia notified ASD on 15 September 2026 → ABC live blog
4. The portal went offline between 14 and 24 September 2026 → live checks on 24 Sep return
   HTTP 503 from the load balancer on every path

## Update and correction (25 September 2026)

- **Time zone.** The Common Crawl timestamps are UTC. The 14 September captures
  (14:03-15:31 UTC) were at 00:03-01:31 on Tuesday 15 September, Canberra time — five days
  after the 10 September email and on the day (not the day before) Services Australia
  notified ASD. The tweet's "still up on September 14" is correct in UTC; in Australian
  time the site was still up on the 15th.
- **What the 200s were.** The HTTP 200 homepage and `mbs_item.jsp` responses are redirect
  stubs to the report form page; the report form page itself was not in that crawl. The
  SAS login service answered with a freshly generated login page. The site was live, but
  point 2's "serving normally" rests on the server and login service answering.
- **Take-down.** The Wayback Machine shows the branded "Service Unavailable" (503) page at
  13:10 on 17 September, Canberra time. The take-down happened between 01:31 on
  15 September and 13:10 on 17 September — whether before or after the minister was told
  on 17 September is not known.
- **The weekend.** 12-13 September was the weekend between the email (Thursday 10th; seen
  by Services Australia on Friday 11th) and the last live capture (early Tuesday 15th).
