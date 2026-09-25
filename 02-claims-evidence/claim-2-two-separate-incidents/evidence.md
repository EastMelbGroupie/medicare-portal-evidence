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

## How to verify

All three sources are live public pages (checked 25 September 2026) — the saved copies are
provided in case they move:
- https://www.abc.net.au/news/2026-09-24/federal-politics-live-blog-openai-medicare-breach/107186578
- https://www.abc.net.au/news/2026-09-24/what-we-know-about-the-openai-medicare-hack/107189452
- https://transluce.org/agent-activity

Caveat: the identification of the breached portal as
medicarestatistics.humanservices.gov.au is the thread author's inference from the
reported details (a SAS-based public statistics portal administered by Services
Australia), not an official statement. Everything else in this folder is sourced.
