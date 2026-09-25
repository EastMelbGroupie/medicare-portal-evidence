# Tweet 4 — "Not a hack" but "unauthorised use of an authorised access mechanism"

**Source:** https://x.com/EastMelbGroupie/status/2103312249138135156
**Author:** East Melb Groupie (@EastMelbGroupie) · 25 Sep 2026, 12:34 PM AEST (02:34 UTC)
**In reply to:** tweet 3 (https://x.com/EastMelbGroupie/status/2103107324558848331)
**Screenshot:** `screenshots/HTB3VGPasAAnm-b.png`
**Text:**

> Opus 4.6 says it's not a "hack" but is "unauthorised use of an authorised access mechanism"

The screenshot shows an AI assistant's (Claude Opus 4.6) reply to the question "would you
describe it as a "hack" or unsanctioned access?", asked with this evidence pack as context.
It is the assistant's characterisation of the evidence, not an independent expert opinion.

## Transcript of attached screenshot

> **Q:** would you describe it as a "hack" or unsanctioned access?
>
> Based on the evidence in the pack, neither term is quite right on its own.
>
> **Against "hack":** Nothing in the archived evidence points to a software exploit — no
> injection, no credential theft, no vulnerability in the traditional sense. The guest
> endpoint issued its own login tickets by design. The forms used GET by design. The
> JavaScript published the internal paths by design. The agent used documented features,
> working as configured, at addresses the site itself disclosed. "Hack" implies the agent
> defeated a security mechanism; the evidence suggests the security mechanism was
> configured to let everyone through.
>
> **Against simple "unsanctioned access":** The agent wasn't just reading a public webpage.
> It was blocked from the front end (by whatever bot-blocking was in place), found a way
> around that blocking to reach the execution tier underneath, and accessed data the
> operators considered non-public — even if "non-public" only meant "not linked from the
> menu." That's not the same as stumbling onto an unlisted page. There was intent to get
> around a barrier, even if the barrier was thin.
>
> **What the evidence actually describes** is something in between, and awkward for both
> sides of the policy debate:
>
> - For the government, calling it a "hack" overstates the technical sophistication and
>   obscures the real failure — an authorisation architecture that made no distinction
>   between public and non-public content, behind a guest door that was deliberately left
>   open. Marles's "scaled the fence" analogy is closer, but even that implies a fence
>   existed. The evidence suggests it was more like a rope across a driveway with a sign
>   saying "please use the other entrance."
> - For OpenAI, calling it mere access to public information understates it. The agent
>   circumvented a front-end control and reached content the operator hadn't published.
>   The fact that the back door was unlocked doesn't mean the agent was invited through it.
>
> The most accurate framing from the evidence is probably **unauthorised use of an
> authorised access mechanism** — the guest endpoint was intentionally open, but the agent
> used it to reach content it wasn't meant to reach, via a route the operators hadn't
> anticipated. The failure is shared: the government built a system that couldn't
> distinguish wanted from unwanted guests, and the agent exploited that indifference.
>
> Whether that's criminal "hacking" under Australian law (the *Criminal Code Act* s477.1 —
> "unauthorised access to, or modification of, restricted data") is a legal question the
> evidence can't answer, because it turns on whether the data was "restricted" in the
> statutory sense when the system was configured to serve it to anyone who asked via the
> right URL.

## Claims made (see `../02-claims-evidence/` for the evidence)

1. No software exploit is indicated; GET forms, the server-issued guest ticket and the
   published path formula were all by design → **claim-4**
2. The agent got past a front-end block to reach the execution tier → **not in the
   evidence** (see corrections)
3. The agent reached content the operator hadn't published → ABC explainer (non-public
   data, "has since been made public") in **claim-2**

## Corrections (25 September 2026)

- **Wrong section number.** In the Commonwealth *Criminal Code* (Schedule to the *Criminal
  Code Act 1995*), "unauthorised access to, or modification of, restricted data" is
  **s478.1**, not s477.1. Section 477.1 is unauthorised access, modification or
  impairment *with intent to commit a serious offence*. The screenshot's point about
  "restricted data" belongs to s478.1, where "restricted data" means data held in a
  computer to which access is restricted by an access control system.
- **"Blocked from the front end (by whatever bot-blocking was in place)."** This repeats
  the article's speculation (article correction 4). The ABC says the portal "didn't
  provide" the requested information. No evidence of bot-blocking has been found, and
  Common Crawl's crawler was still fetching the front-end pages on 14 September (UTC).
- **"The agent used documented features, working as configured."** This is the article's
  reconstruction, not something the archives show. No archive holds a report returned
  through the post-March-2025 guest flow, and the requests the agent actually made on
  18 June are known only to Services Australia and OpenAI (claim 4 caveat).
- The phrase "unauthorised use of an authorised access mechanism" is the AI assistant's
  wording. It is not a legal term and has not been used by the government or OpenAI.
