# Evidence package: the OpenAI agent breach of the Medicare statistics portal

**Prepared:** 25 September 2026 · **Purpose:** the supporting evidence for the items cited in
the @EastMelbGroupie X article and tweets

**A note on times:** archive timestamps are in UTC. Canberra time in September 2026 is
AEST (UTC+10), so an archive capture late on 14 September UTC is early on 15 September in
Australia. Both are given below where it matters.

**New to the issue?** Start with [`ABOUT-THE-ISSUE.md`](ABOUT-THE-ISSUE.md), a two-page
summary of what the public records add to the story.

## Reading this repository with an AI assistant

This project is designed to be read by AI assistants and large language models (LLMs) as
well as by people. Every claim has its own folder with an `evidence.md` that states the
claim, its evidential status (proven / strongly indicated / inference), the supporting
files, and the limits. The raw records (archive indexes, captured pages, code, saved
articles) sit alongside as plain text, so a model can check the write-up against the
source.

Point your LLM at this repository and ask it questions such as:

- "What evidence supports the claim that the portal was still online on 15 September?"
- "Is this an exaggeration, or does the evidence back it up?"
- "Which parts of this are proven, and which are inference?"
- "What would it take to disprove finding 3?"
- "Do the corrections in the tweet files change the conclusions?"
- "Check the Common Crawl records in claim 1 against the timeline in the README."

Ask it to cite file paths for every answer, and to separate what the records show from
what the author infers.

**A note on models:** GLM-5.3 was used in preparing this analysis because it will discuss
hacking issues directly. Some models will only discuss hacking from a public policy point
of view.

---

## The story in one line

Independent public web archives (Common Crawl, Wayback Machine) show the government
Medicare statistics portal hacked by an OpenAI agent on 18 June 2026 was **still online in
the early hours of Tuesday 15 September 2026, Canberra time** (14 September UTC) — five
days after OpenAI's 10 September disclosure email, on the day Services Australia notified
ASD, and nearly 13 weeks after the breach. By 1:10pm on Thursday 17 September it was
serving a "Service Unavailable" page.

## The three findings

1. **The exposure window was nearly three months, not eight weeks.** The Wayback Machine
   captured the portal's report form page serving normally on 26 June and 11 August 2026.
   Common Crawl captured the site's web server and SAS login service live and answering
   (HTTP 200) on 19 July, 18 August and **00:03-01:31 on 15 September 2026, Canberra time**
   — after OpenAI's disclosure email. The first archived "Service Unavailable" (HTTP 503)
   page is 17 September 2026, 1:10pm Canberra time. (Folder 2, claim 1.)
2. **The breached portal appears in none of the publicly released agent-activity records** —
   unlike the AIHW probing of 20-21 June, which Transluce documented from public urlquery.net
   records. Note what this does and doesn't show: public urlquery.net searches find no scan
   of the portal before the 24 September announcement, but Transluce's dataset never
   searched for this domain, so its silence is neutral. The June 18 breach is known only
   from the parties' internal records. (Folder 2, claims 2 and 3.)
3. **The portal's reporting interface was built on plain GET requests with a guest
   (no-password) login** — its forms used `method="get"`, its own JavaScript published its
   internal addressing, and its guest endpoint issued its own login ticket. On the archived
   evidence no software exploit is indicated; the likelier failure is authorisation
   architecture. Before March 2025 it is proven: full reports were served to anonymous
   GETs for a decade. After March 2025 it is strongly indicated: a public R scraper
   published on GitHub on 27 March 2025 retrieved report tables with plain,
   credential-free GETs to the guest endpoint. Its code parses the returned tables, and its
   notes record the server's behaviour. Caveats: no archive or committed output captures a
   post-2025 report response (archived guest calls end in redirect loops or at a login
   page, because the flow needs cookies), and the scraper shows public statistics only,
   not the non-public content. (Folder 2, claim 4; folder 4.)

## What's in this package

```
ABOUT-THE-ISSUE.md         Plain-language summary of the three findings and why they matter

01-article-and-tweets/     The published analysis, as posted on X (24-25 Sep 2026)
  article-the-openai-hack-that-wasnt.md   — full X article, markdown (+ corrections note)
  tweet-1-two-hacks-compared.md           — "similar but different" tweet + OCR of screenshot
  tweet-2-why-no-trace-in-logs.md         — image-only tweet + OCR of screenshot
  tweet-3-sept-14-still-online.md         — "still up on Sept 14" tweet + OCR of screenshot
  tweet-4-not-a-hack.md                   — "unauthorised use of an authorised access
                                            mechanism" (AI assistant's framing) + transcript
  tweet-5-private-company-comparison.md   — AI assistant's legal comparison with a private
                                            company + transcript (commentary, not evidence)
  screenshots/                            — the five original tweet screenshots (PNG)

02-claims-evidence/         One folder per claim, each with an evidence.md, the underlying
                           files, and "how to verify" steps that take minutes and require
                           no special access:
  claim-1-portal-live-until-sept-14/   — Common Crawl index dumps Apr-Sep 2026, the
                                         14 Sep response bodies, Wayback capture indexes
  claim-2-two-separate-incidents/      — saved ABC live blog + ABC explainer + Transluce report
  claim-3-no-released-data-for-june-18/— urlquery.net search results record
  claim-4-portal-architecture-get-only/— archived portal page, JavaScript, redirect chain,
                                         Wayback capture index of the SAS endpoints,
                                         bfiripis-github-scraper/ (Mar 2025 public R
                                         scraper of the guest endpoint)

03-web-archive-captures/   Archived pages of the portal itself (Wayback Machine):
                           Apr 2026 report form (method="get"), SetupEnvironment.js,
                           a 2015 anonymous report, post-breach captures (26 June,
                           11 Aug 2026) showing the form page still normal, and archived
                           guest-endpoint redirect chains (Nov 2025, May 2026)

04-get-only-access/        GET-only-access-evidence.md — the technical evidence on how the
                           portal was addressed (GET forms, guest login), with its limits

05-source-documents/       The long-form investigation article. (The Transluce dataset,
                           38,160 records, is not included because email blocks nested
                           zips: download it from https://transluce.org/data/urlquery-agent-activity-2026-09-23.zip
                           — SHA-256 969a13fbd7d80d7e1556eef58a347f52ecdd85661c541f6c0d1d6f5e2a86570d)
```

## How to verify the headline finding in under 5 minutes

1. Open: `https://index.commoncrawl.org/CC-MAIN-2026-39-index?url=medicarestatistics.humanservices.gov.au&matchType=domain&output=json`
2. See rows timestamped `20260914...` (UTC) with `"status": "200"` — the site answering on
   14 September UTC / 15 September Canberra time. (The Common Crawl index server is
   intermittently overloaded; if it returns 502/504, retry.)
3. Open: `https://www.abc.net.au/news/2026-09-24/federal-politics-live-blog-openai-medicare-breach/107186578`
   and search "Timeline of breach": OpenAI's email was 10 September; ASD notified 15 September;
   minister told 17 September.
4. Open: `https://web.archive.org/cdx/search/cdx?url=medicarestatistics.humanservices.gov.au&matchType=domain&from=20260901`
   — from `20260917031054` (17 September, 1:10pm Canberra time) the captures return 503.
5. Optionally visit `https://medicarestatistics.humanservices.gov.au/` — every path still
   returns 503.

Every claim traces to a public, independently checkable archive.

## Important caveats

- The identification of the breached site as **medicarestatistics.humanservices.gov.au**
  is the analysis author's **inference** from the reported details (a public Medicare and
  PBS statistics portal administered by Services Australia; Minister Gallagher: "It's a
  public website. It's most often used by researchers and academics"). The PM did not name
  the site. (The phrase "unpatched legacy systems" in coverage comes from opposition
  defence spokesperson James Paterson, not from the government.)
- The reconstruction of the incident mechanism (GET-only access, authorisation-architecture
  failure) is inference from public records. A third-party scraper (March 2025) strongly
  indicates the post-2025 guest flow returned public reports to plain GETs, but no
  archived response shows it, and nothing public shows how non-public content was
  reached. The actual request logs are held by Services Australia and OpenAI; the ongoing taskforce/ASD investigation is the authoritative source for what
  happened on 18 June.
- The exact take-down time is known only to within a window: the site was answering at
  01:31 on 15 September and serving a 503 page by 13:10 on 17 September (Canberra time).
  The minister was told on 17 September; whether the take-down came before or after that
  is not known.
- The reported details "found a security workaround", "blocked from the data" and "wrote
  files to the server" come from secondary reporting that is not archived in this pack; the
  ABC sources here say the portal "didn't provide" the requested information before the
  agent "gained unauthorised access" (Marles: "It sought information, information was not
  given, and then it effectively hacked into that medical portal").
- The archived records were obtained passively from public archives (Wayback Machine,
  Common Crawl, urlquery.net). No testing or probing of any live system was performed.
- Archived calls to the portal's guest endpoint exist from March 2025 onward (including
  Oct-Dec 2025, Feb 2026, 5 and 7 May 2026, and 4 June 2026). They are most likely archive
  crawler captures of the site's own form target; who triggered them is unknown, and they
  should not be presented as reconnaissance.

## Attribution

The analysis was published on X by @EastMelbGroupie (article 24 Sep 2026, ~51K views;
follow-up thread same day). This package is meant to stand on its own: every claim cites
public records, and each evidence file includes steps to verify it independently, which is
encouraged and expected. The published article and tweets are reproduced as posted;
corrections are appended to each file.

## Version history

- **25 Sep 2026, v1** — first version.
- **25 Sep 2026, v2** — added tweets 4 and 5 (posted 12:34 and 12:43 AEST, 25 Sep, after
  v1 was sent). Both are screenshots of an AI assistant's (Claude Opus 4.6)
  characterisation of the evidence: whether the incident is a "hack", and how a private
  company with the same configuration would be treated. They are commentary, not
  evidence. Their corrections sections fix a wrong Criminal Code section number (s478.1,
  not s477.1) and note that privacy-law points do not apply where no personal data was
  involved.
- **25 Sep 2026, v2 (cont.)** — added `02-claims-evidence/claim-4-portal-architecture-get-only/bfiripis-github-scraper/`:
  a public R scraper (GitHub repo created 27 March 2025, surfaced by @foilmanhacks on
  24 Sep 2026) that calls `/SASStoredProcess/guest` with plain GETs and no credentials.
  Finding 3 and the claim-4 caveats are upgraded from "inference, not proof" to "strongly
  indicated, not captured" for public reports after March 2025. Section 6 was added to
  `04-get-only-access/GET-only-access-evidence.md`. Nothing about non-public access changed.
- **Public repository release** — identical to v2 except that two response headers, `x-nid`
  and `x-as`, are removed from the three `chain_*.txt` redirect traces. The Wayback
  Machine adds these headers to identify the network of whoever makes the replay request,
  so they describe the compiler's internet connection, not the portal. No archived portal
  header, URL or timestamp was changed. For privacy, the scraper author's personal name is
  also withheld. The LinkedIn screenshot, its tweet-image links and the LinkedIn post ID are
  removed, and the name is replaced on three commits in `git-log.txt`
  (see `bfiripis-github-scraper/evidence.md`).
