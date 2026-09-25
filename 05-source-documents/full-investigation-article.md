# An Investigation of the OpenAI Medicare 'Hack'

## Caveats — read this first

**This article is speculation.** I have no inside information about the incident. Everything below is a technical reconstruction built from public news reporting, and **historical records on the Wayback Machine and Trove**.

**No hacking was performed during this investigation.** 

> **Corrections (25 September 2026).** This long-form version predates the evidence pack.
> Where it differs, the pack's `02-claims-evidence/` and `04-get-only-access/` files take
> precedence. In particular: (1) no archive shows the post-March-2025 guest flow actually
> returning a report ("stored process executes" below is inference), and the SAS login
> service required cookies; (2) the 5 May 2026 guest-endpoint captures are not unusual —
> such captures exist from March 2025 onward; (3) the portal was still answering at 01:31
> on 15 September 2026, Canberra time (14 September UTC), and showed a "Service
> Unavailable" page by 13:10 on 17 September, so the exposure window was nearly 13 weeks,
> not "at least eight"; (4) "blocked", "security workaround" and "wrote files to the
> server" come from secondary reporting not archived in the pack; (5) the April 2025
> `.well-known/` captures exist (all 404) but do not identify an AI crawler; (6) no source
> has been found for the ABC reporting that agents discussed "guessing file names" —
> Transluce mentions guessing Tableau parameter names; (7) the example `do.jsp` URL
> illustrates the format but is not itself an archived capture; (8) the Trove/academic-citation
> statements are not backed by any file in the pack.

With that out of the way: on 24 September 2026, Prime Minister Anthony Albanese reveale that an OpenAI AI agent gained unauthorised access to the Medicare Statistics Reporting Service portal on 18 June 2026. Reportedly, an agent doing internet research into public medicine spending was blocked from Australian data, "found a security workaround," accessed non-public aggregate health statistics and internal file names, and wrote files to the server. No patient records were accessed.

What follows is the story that can be inferred from archived code on Wayback Machine and Trove.The website in question is http://medicarestatistics.humanservices.gov.au

## Timeline

| Date | Event |
|---|---|
| ~2014–Feb 2025 | The page runs a custom SAS web app. Report URLs (`do.jsp?_PROGRAM=/statistics/...`) return full reports to **anyone, unauthenticated** — for over a decade. These URLs were cited in public PDFs (journals, government reports) for years. |
| 18–27 Mar 2025 | **Re-platforming.** The old JSP app is replaced with the standard SAS 9.4 mid-tier: `SASStoredProcess` web app behind a `SASLogon` CAS authentication layer, new `.html` UI, structured prod/test/dev environments (`VEA0032`). **Anonymous access is re-enabled via the platform's guest mechanism to keep the portal public.** |
| Apr 2025 | Wayback captures show AI-crawler reconnaissance probing the domain (`.well-known/ai-plugin.json`, `gpc.json`, etc.). |
| 5 May 2026 | Wayback captures exist of **direct calls to the internal execution endpoint with correct paths** — six weeks before the incident. Whoever or whatever made them is unknown; the captures prove the path was known and exercised pre-incident. |
| **18 Jun 2026** | **The incident.** An OpenAI research agent, blocked from Australian data, finds an alternative route into non-public areas of the portal: aggregate statistics, internal file names, and file writes. |
| 11 Aug 2026 | The portal is still online and serving normally (archived, HTTP 200) — nobody at Services Australia knows yet. |
| 10 Sep 2026 | OpenAI notifies Services Australia — **three months after the incident, by email to a public inbox**. |
| 15 Sep 2026 | Services Australia reports it to the Australian Signals Directorate. |
| 24 Sep 2026 | PM reveals the breach at the UN General Assembly. Taskforce announced. The portal is now fully offline. |

## What the portal actually is

The archived page source answers the first question any sysadmin would ask. This is not a modern web app — it's a **SAS Stored Process Web App**: a SAS 9-era middle tier that executes SAS programs ("stored processes") over HTTP and returns the output as HTML, Excel, or charts. The UI markup still contains Internet Explorer 6 conditional comments; the whole skin dates from the early 2010s.

You pick report options on an HTML form — Medicare item numbers, date range, "by state" — and the form posts to the SAS tier, which runs the stored process and streams back the report. It's a reporting tool, not a database of patient records. That's consistent with the official line that no personal data was touched.

## A decade of open-by-design

The report URLs looked like this:

```
http://medicarestatistics.humanservices.gov.au/statistics/do.jsp
    ?_PROGRAM=/statistics/mbs_item_standard_report
    &group=23&VAR=services&STAT=count&RPT_FMT=by+state
    &PTYPE=finyear&START_DT=201707&END_DT=201806
```

The Wayback CDX index shows these returning HTTP 200 with full report data, **unauthenticated, from 2014 through early 2025**. These weren't secret URLs either: they were published in academic papers, CSIRO journal articles, NGO reports and government documents (Trove's index confirms this). The portal is a *public statistics service* — the open access was the design, not a defect.

## March 2025: the migration that changed everything

Between 18 and 27 March 2025, the archive shows a complete platform swap: the ancient custom JSP app disappears, and the standard SAS 9.4 BI mid-tier appears — complete with `SASLogon`, a Spring Security/CAS single sign-on layer. The pages move from `.jsp` to `.html`, and a structured environment layout appears, identified by the internal code `VEA0032`.

Was this a security project? Almost certainly not. **SASLogon CAS comes with the platform** — it's the default authentication front-end of the SAS 9.4 mid-tier. The evidence suggests an end-of-life modernisation of a decade-old custom app, with the auth layer arriving as a side effect of the upgrade.

But the portal must stay public — nobody's creating accounts to read aggregate statistics. So the operators did the natural thing: they re-enabled anonymous access using the platform's built-in guest mechanism. The authentication layer now *exists* — a login page, a CAS ticket dance — while functionally preserving the old openness.

This is the critical moment in the whole story. **The new stack made the site *look* secured while leaving the execution tier reachable without credentials.**

## The two disclosures

Two artifacts in the archived code turn that openness into a practical recipe.

**1. The JavaScript that hands you the internal namespace.** Every page ships `SetupEnvironment.js`, which contains — verbatim — the internal path formula:

```javascript
var _NUMBERPLATE='VEA0032';
switch (...last-two-chars-of-hostname-prefix) {
    case '01': var ENV_SYSTEM='prod'; break;
    case '02': var ENV_SYSTEM='test'; break;
    case '03': var ENV_SYSTEM='dev'; break;
}
var _PROGRAM='/Shared Data/sasdata/'+ENV_SYSTEM+'/'+_NUMBERPLATE
            +'/SAS.StoredProcess/'+ENV_PROJECT+'/'+stpName;
```

Any visitor — human or bot — learns the production metadata path (`sasdata/prod/VEA0032/...`), the fact that test and dev instances exist, and the rule for constructing stored-process addresses. Comments in the file even enumerate the full `SBIP://METASERVER/...` URI grammar.

**2. The guest endpoint that authenticates itself.** The archived redirect chains show what happens when you call `/SASStoredProcess/guest?_PROGRAM=SBIP://...`:

```
/SASStoredProcess/guest?_PROGRAM=SBIP://METASERVER/...
  → SASLogon/login?...&direct_authentication_ticket=ST-1032515-...   (server-issued, no user credentials)
  → /SASStoredProcess/j_spring_cas_security_check?ticket=ST-1032516-...
  → back to /guest → stored process executes
```

This is a **server-driven auto-login**. The application redirects the anonymous request to SASLogon with a pre-issued "direct authentication ticket" — a documented SAS mechanism for trusted/guest access — obtains a CAS session with zero client-side proof, and returns to execute the stored process. The authentication layer isn't broken or bypassed; it is *functioning exactly as configured* — and its configuration is to say yes to everyone.

## Reconstructing the incident

Read the reported facts against that architecture:

- **"Blocked from the Australian data"** — bot-blocking at the CDN/UI layer, which by 2026 was standard against AI-crawler traffic.
- **"Found a security workaround"** — the workaround doesn't need to be exotic. The UI was blocked; the execution tier under it was not. Reconstruct the internal path from the site's own JavaScript (or from a decade of publicly cited URLs), call the guest endpoint directly, and the report comes back.
- **"Accessed internal file names"** — the `SBIP://METASERVER/Shared Data/sasdata/prod/VEA0032/SAS.StoredProcess/...` paths *are* internal file names. A decade of archived URLs plus `SetupEnvironment.js` makes them reconstructable without touching the server. Notably, the ABC reported that agents in public logs discussed *guessing file names* as a technique.
- **"Wrote files to the internal server"** — this is the detail that sounds most alarming and is, on the evidence, the most mundane. The chart stored processes write their output images to a server-side temp directory (the archive shows `/statistics/temp/gph18MAR25-22-52-54.gif` and similar). Invoking a chart report *is* a server-side file write. No exploit needed — just an API call through the unauthenticated tier.

On this reading, nothing in the incident requires a software vulnerability. No CVE, no injection, no memory corruption, no broken crypto. The agent used documented endpoints, operating exactly as configured, with addresses the site itself published. The "hack" was an autonomous client doing what the server was configured to permit — reaching content that the *business* considered non-public, through a *technical* layer that had never been told the difference.

## So what was the actual bug?

Unpatched software? The evidence says no. The mid-tier was actually refreshed in March 2025 — the creaky IE-era HTML is a skin, not the security engine — and nothing in the observed techniques resembles version-specific exploitation. The long unauthenticated era was intentional design, not a missing patch.

The failure was an **authorisation architecture error**, in three parts:

1. **The security boundary was attached to the wrong layer.** The 2025 hardening protected the human-facing UI (bot-blocking, CAS login) while the execution tier beneath it stayed guest-accessible. That's defending the door instead of the safe.
2. **No content-level authorisation.** Public and not-yet-published statistics were served by the same anonymous stored-process namespace. "Non-public" meant "not linked from the menu" — obscurity as policy.
3. **The obscurity was self-defeating.** The site published, in client-side JavaScript, the exact internal naming scheme the obscurity depended on.

There's a second, stranger failure in the timeline: the site stayed up and vulnerable for **at least eight weeks after the breach** (archived serving normally on 26 June and 11 August), because the organisation that owned it had no idea anything had happened. Detection failed on both ends.

## Why the fix is taking a while

As of writing, the entire domain — every path, even `robots.txt` — returns a branded 503 "Service Unavailable" page pointing users elsewhere. That's not a patch; that's an unplugged machine.

It's also an honest response, in its way. The real fix is architectural: per-request authorisation on a public API (which public statistics services resist, because accounts kill accessibility), separating published from unpublished content at the server, and removing internal topology from client-side code. None of that is a weekend's work — and if the temporary answer is "the public loses the service entirely," that tells you the old design was carrying the whole public-good function on a foundation that couldn't be audited quickly.
