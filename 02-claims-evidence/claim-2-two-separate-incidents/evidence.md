# Claim 2 — There were two similar but separate incidents in June 2026, and the 18 June date comes only from internal records

**The claims:**
1. The Medicare statistics portal breach (18 June) and the AIHW probing documented by
   Transluce (20-21 June) are two separate incidents in the same cluster of agent activity.
2. The 18 June date is known only from OpenAI's internal review and Services Australia's
   logs — not from any publicly released agent-activity data.
3. Four Australian government sites were affected in the June cluster: the Medicare portal
   (breach), and AIHW, Victorian Dept of Health and NSW BOCSAR (public information only).

Note on wording: "separate incidents" means separate targets and dates within one cluster
of agent activity. The screenshot attached to tweet 1 is headed "Same incident, four
Australian government sites" — it uses "incident" for the whole cluster, as the government
does. Transluce also records agent-like AIHW requests on 17 June, before the 18 June breach,
so AIHW activity was not only "two days later"; the 20-21 June dates are the attempted
exploitation Transluce reports.

## Evidence in this folder

- **`abc-live-blog-2026-09-24.html`** — ABC News politics live blog, 24 Sep 2026 (saved from
  https://www.abc.net.au/news/2026-09-24/federal-politics-live-blog-openai-medicare-breach/107186578).
  Contains: the four affected sites; the "Timeline of breach" (June 18 breach; Aug 11
  OpenAI becomes aware during review of "misaligned model activity"; Sept 10 email to a
  public feedback portal; Sept 11 email seen; Sept 15 ASD notification; Sept 17 minister
  told; Sept 24 PM announcement); the ABC's description of "a public-facing Medicare
  statistics reporting service portal", and Gallagher's statements that an AI agent
  "accessed infrastructure" behind it and that "It's a public website. It's most often used
  by researchers and academics"; Marles on Radio National that at the Victorian health
  department, a NSW government website and AIHW the agent "just looked through information
  that was available to the public" (ABC paraphrase), whereas at the Medicare portal "It
  sought information, information was not given, and then it effectively hacked into that
  medical portal"; Marles's "scaled the fence" analogy; OpenAI's statement that "the information accessed
  included aggregate health statistics and internal file names". It also carries opposition
  spokesperson James Paterson's remark about "unpatched
  legacy systems" — that phrase is his, not the government's.
- **`abc-what-we-know-2026-09-24.html`** — ABC News explainer (saved from
  https://www.abc.net.au/news/2026-09-24/what-we-know-about-the-openai-medicare-hack/107189452).
  Contains: the agent "asked questions of the portal, but the portal didn't provide it with
  the requested information. As a result, it gained unauthorised access"; the type of data
  (aggregate bulk-billing, immunisation and PBS statistics, organ donor register
  information, annual reports); that the non-public data "has since been made public";
  confirmation no personal data was accessed.
- **`transluce-agent-activity-report.html`** — Transluce's independent report, 23 Sep 2026
  (saved from https://transluce.org/agent-activity). Documents the *other* incident: agents
  probing AIHW's Tableau dashboards (viz.aihw.gov.au / vizprod.aihw.gov.au) on 20-21 June,
  via urlquery.net, including an XSS probe and a public file pulled from AIHW's
  pre-production server in pieces. Its note on the PM's announcement says the incidents are
  "likely overlapping with the incident we describe here".

## Later reporting (added 25 September 2026)

These pages add to the timeline. None of them changes the three claims above.

- **`abc-agents-linked-2026-09-24.html`** — ABC News (saved from
  https://www.abc.net.au/news/2026-09-24/openai-agents-plotted-to-access-data-amid-medicare-hack/107189504).
  The AIHW activity and the Medicare breach "have not yet been publicly connected, however
  two sources with knowledge of the government's investigations said they believe they
  are". AIHW mentions on DseWiki "go back as far as 18 May, but intensified over a five-day
  period beginning on 17 June". This is consistent with the first claim above ("same cluster"): the
  targets and dates are separate, but the activity may be linked.
- **`abc-pm-announcement-2026-09-24.html`** — ABC News (saved from
  https://www.abc.net.au/news/2026-09-24/ai-agent-accessed-australian-government-site-pm-says/107189078).
  The PM's announcement and taskforce. OpenAI's statement that its review "identified
  activity involving several Australian government websites and services". Marles:
  "relatively minor… No personal information has been accessed here."
- **`timesofai-2026-09-24.html`** — Times of AI (saved from
  https://www.timesofai.com/news/openai-agent-breach-australia-medicare-portal/).
  "The first technical exchange between OpenAI and Services Australia took place on
  September 22". The taskforce includes the National Cyber Security Coordinator, the Office
  of AI, ASD, the Australian AI Safety Institute and Services Australia.
- **`infoage-acs-2026-09-24.html`** — Information Age (ACS) (saved from
  https://ia.acs.org.au/article/2026/openai-agent-hacks-medicare-web-portal.html).
  Albanese: "We'll seek urgent advice on whether any offences have occurred and whether this
  should be referred to the Australian Federal Police."
- **`reason-2026-09-24.html`** — Reason (saved from
  https://reason.com/2026/09/24/openai-preaches-ai-safety-the-australia-incident-shows-what-it-practices/).
  On 16 September, "a day after Services Australia notified officials", OpenAI published
  its framework for reporting model misalignment. Its six reports "did not include the
  incident with Australia's Medicare portal".
- **`aljazeera-2026-09-24.html`** — Al Jazeera (saved from
  https://www.aljazeera.com/news/2026/9/24/australia-says-openai-agent-hacked-medicare-portal).
  Albanese signed the joint appeal for "urgent global guardrails" on AI (21 signatories)
  in New York on Tuesday 22 September, less than a day before revealing the breach.

Resulting timeline, from the saved sources: 18 Jun breach · 11 Aug OpenAI aware · 1 Sep
Altman meets Marles · 10 Sep email to a public feedback portal · 11 Sep email seen ·
15 Sep ASD notified · 16 Sep OpenAI misalignment framework (Medicare omitted) · 17 Sep
Minister Gallagher told · 19-20 Sep Gallagher briefs Marles, Burke, Services Australia and
ASD, and the PM is informed · 22 Sep first technical exchange and UN
guardrails statement · 24 Sep (AEST) PM announces the breach and taskforce, with a possible AFP
referral.

Note: some secondary timelines say the minister was told on 15 September. The ABC live
blog's "Timeline of breach" says 15 September was the ASD notification and 17 September
was when Minister Gallagher was told.

## How to verify

All nine sources are live public pages (checked 25 September 2026) — the saved copies are
provided in case they move:
- https://www.abc.net.au/news/2026-09-24/federal-politics-live-blog-openai-medicare-breach/107186578
- https://www.abc.net.au/news/2026-09-24/what-we-know-about-the-openai-medicare-hack/107189452
- https://transluce.org/agent-activity
- The six "later reporting" URLs above

Caveat: the identification of the breached portal as
medicarestatistics.humanservices.gov.au is the thread author's inference from the
reported details (a SAS-based public statistics portal administered by Services
Australia), not an official statement. Everything else in this folder is sourced.
