# Tweet 5 — A private company with this configuration "would be in trouble"

**Source:** https://x.com/EastMelbGroupie/status/2103314512933962225
**Author:** East Melb Groupie (@EastMelbGroupie) · 25 Sep 2026, 12:43 PM AEST (02:43 UTC)
**In reply to:** tweet 4 (https://x.com/EastMelbGroupie/status/2103312249138135156)
**Screenshot:** `screenshots/HTB5ZczbYAAE7ie.png`
**Text:**

> It also says a private company with this configuration would be in trouble if any
> private data leaked.

The screenshot continues the same AI assistant (Claude Opus 4.6) conversation as tweet 4.
It is general commentary by an AI model, not legal advice or an expert opinion.

## Transcript of attached screenshot

> Under current Australian law, a private company with this configuration would face
> serious regulatory exposure:
>
> - **Privacy Act (APP 11):** Penalties up to $50M or 30% of turnover for failing to take
>   "reasonable steps" to protect personal information — though only if personal data was
>   involved, not just aggregate statistics.
> - **Notifiable Data Breaches scheme:** Three months with no detection would draw scrutiny
>   on monitoring adequacy.
> - **SOCI Act:** Health-sector entities have mandatory cyber risk management obligations
>   post-2022.
> - **Common law negligence and consumer law:** The gap between the login page (looks
>   secure) and the guest endpoint (lets everyone through) is hard to defend as reasonable,
>   and could be misleading conduct if security was represented to users.
>
> The architecture is particularly indefensible because no exploit was needed — documented
> features working as designed, internal paths published in JavaScript, no monitoring for
> three months.
>
> The uncomfortable part for the policy debate: the government is discussing charging
> OpenAI, but if a private company were on both sides of this, a court would likely note
> the custodian's own negligence. That said, "the door was open" isn't a legal defence to
> computer offences in Australia — the configuration being negligent and the access being
> unauthorised can both be true simultaneously.

## Claims made

This tweet is legal and policy commentary. The evidence folders do not cover it. The
factual premises it relies on are:

1. No exploit was needed, and the site published its internal paths in JavaScript →
   **claim-4** (the path formula is proven; that no exploit was used is inference)
2. The breach went undetected for about three months → ABC live blog timeline in
   **claim-2** (OpenAI became aware on 11 August and emailed on 10 September; Services
   Australia learned of it from OpenAI, not from its own monitoring)
3. No personal data was accessed → ABC explainer in **claim-2**

## Corrections and qualifications (25 September 2026)

- **The Privacy Act and the NDB scheme do not apply on the reported facts.** Both concern
  personal information. The government says only aggregate statistics were accessed. The
  screenshot concedes this for APP 11, but the Notifiable Data Breaches point has the same
  limit. Also, Services Australia is itself bound by the Privacy Act as an agency, so the
  "private company" comparison is not a contrast on privacy law.
- **SOCI Act.** Its risk-management obligations apply to specified critical
  infrastructure assets (such as critical hospitals). Whether a Commonwealth statistics
  portal would be covered has not been checked, and Commonwealth agencies are mainly
  governed by the Protective Security Policy Framework instead. Treat this point as
  unverified.
- **"No monitoring for three months."** What the record shows is that the breach was not
  detected for about three months, because Services Australia learned of it from OpenAI.
  Whether any monitoring existed is not known.
- **"No exploit was needed."** This is inference from archived records; see the claim-4
  caveat.
- The Privacy Act penalty figure is the maximum for a body corporate's serious
  interference with privacy (the greater of $50 million, three times the benefit
  obtained, or 30% of adjusted turnover). Nothing in the pack verifies it.
