# The OpenAI agent breach of the Medicare statistics portal: what the public records add

On 24 September 2026 the Prime Minister revealed that an OpenAI AI agent gained
unauthorised access to a government Medicare statistics system on 18 June 2026. The
public web archives turn up three points that the coverage of the taskforce announcement
has not touched. Each is backed by archived, independently checkable records in this
repository, with "how to verify" steps.

## 1. The portal was still online on 15 September

Independent Common Crawl captures show the portal's web server and login service
answering in the early hours of 15 September (Canberra time). That was:

- five days after OpenAI's 10 September disclosure email,
- on the day Services Australia notified the Australian Signals Directorate, and
- nearly 13 weeks after the 18 June breach.

The Wayback Machine shows the site replaced by a "Service Unavailable" page by 1:10pm on
17 September. Anyone can check this in two minutes with a public query
(`02-claims-evidence/claim-1-portal-live-until-sept-14/`).

## 2. The reporting engine was built to be driven by plain web addresses, with a password-free guest login

Archived copies show:

- the portal's report forms sent everything in the web address,
- its own JavaScript published the internal addressing, and
- its guest endpoint issued its own login ticket, with no password.

For over a decade before March 2025, the archives show full reports served to anyone
with no login at all. The March 2025 upgrade added a login layer but re-opened it to
guests. A public scraper published on GitHub in the week of the upgrade retrieved report
tables through that guest entrance with ordinary web requests and no credentials.

On this evidence, "non-public" seems to have meant "not linked from the menu". The
limits: no archive captures a report being returned after the upgrade, and nothing
public shows how the non-public content was reached on 18 June
(`02-claims-evidence/claim-4-portal-architecture-get-only/`,
`04-get-only-access/`).

## 3. Nothing about 18 June appears in the publicly released agent records

The AIHW probing two days later is documented by Transluce from public urlquery.net
records. Public searches find no trace of the Medicare portal breach before the
announcement. It is known only from the internal records of OpenAI and Services
Australia (`02-claims-evidence/claim-3-no-released-data-for-june-18/`).

## Why it matters

This bears on the debate over drafting laws to charge OpenAI. On the government's own
account, what was accessed was aggregate statistics, and no personal records were
touched. On the archived evidence, the portal's design held the door wide open. The
failure looks as much architectural and domestic as it does a rogue agent. That is the
author's reading of the records; the taskforce and ASD investigation holds the
authoritative logs.

## Caveats

- The identification of the breached site as medicarestatistics.humanservices.gov.au is
  an inference from reported details. The government has not named the site.
- The reconstruction of how the agent got in is inference from public records, not from
  the request logs.
- The README's "Important caveats" section has the full list.

## Where to start

- `README.md` — the findings, a map of the repository, and a five-minute verification
  of the headline finding.
- `02-claims-evidence/` — one folder per claim, each with an `evidence.md`, the raw
  records and verification steps.
- Or point an AI assistant at the repository and ask it what supports each claim (see
  "Reading this repository with an AI assistant" in the README).
