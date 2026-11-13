# Playbook — operating rules for the application pipeline

Sanitized public version. The live playbook also contains a standard
identity block (name, contact, work authorization for two countries,
education, current resume pointer) that every form is filled from verbatim,
and a standing-answers table (§8). Personal values are not published; the
rules are the point.

Every rule below exists because of a dated failure. The run reports cite the
rule, not the anecdote — but the anecdotes are why nobody argues with the
rules.

## 1. Standing decisions (decide, don't block; flag everything in the report)

Low-stakes form fields are decided by rule so they never stall a run:

- **Source / "how did you hear"**: "Job board" if offered, else "Other" —
  never claim a specific named board that wasn't the actual channel.
- **Relocation / travel willingness**: Yes, as an interview-stage signal;
  flagged for later confirmation.
- **Self-ID / EEO questions**: decline each, or leave blank where truly
  optional.
- **Required-but-inapplicable fields**: enter "N/A" rather than leaving them
  blank — a blank required field blocks submission, and entering a past
  employer as if current would be a false statement. ("Current company"
  while between roles is the standing example.)
- **Consent checkboxes** (privacy acknowledgement): Yes. Marketing opt-ins:
  No.

## 2. Hard stops (never cross, even to complete an application)

- Never invent credentials, employment history, metrics, or legal
  attestations. Background checks verify them.
- Never claim certifications not held, people-management experience, or
  domain experience the resume doesn't evidence. State gaps plainly in cover
  letters instead.
- Novel legal attestations — anything outside the standing-answers table —
  hold that single application for the human. The run continues.
- Forms that reject the candidate's actual location/timezone eligibility:
  stop; never fudge location.
- New account creation: never without the operator's explicit approval.
  Account-gated applications are skipped and logged.
- $0 spending, always.

## 3. Salary rules (historically the biggest source of errors)

- The **live posting's** published range governs. Prep files and briefs go
  stale; re-check the live posting every time. (Brief-stated ranges were
  wrong twice in the first run — one invented outright, one superseded by
  the published range.)
- If no figure is published and none is authorized: invent nothing. A text
  field gets "Open to discussion" (or "Open to discussion within the posted
  $X–$Y range" when a range exists). A numeric-only field gets the published
  range's midpoint, or holds if there is no published range.
- Every estimated or below-floor figure is flagged in the report.

## 4. Platform patterns (what breaks where)

- **Email verification codes** (Greenhouse and others): codes are
  per-application — never reuse one. The code is retrieved from the mailbox
  through a protected read and shared only with the operator's approval on a
  confirmation card.
- **Hung tabs**: a frozen tab after Submit does NOT mean failure. Always
  check the mailbox for the employer's thank-you email before re-entering
  anything. (One tab hung with no confirmation page while the receipt was
  already in the inbox; resubmitting would have duplicated the application.)
- **Captcha walls** (Lever's, in practice): the operator has granted standing
  permission to solve captchas on his own applications, in persistent good
  faith. Some platforms reject even correctly solved challenges — after real
  effort, the application is logged as captcha-unsolved with its prep work
  preserved for manual submission, and the run moves on. Never burn a run
  looping on one form.
- **Upload-only cover letters** (Ashby): submit without one rather than
  fabricating or converting a file; flag it.
- **Auto-parsing forms** (Rippling): parsers sometimes fill "current
  company" from the resume's most recent employer. If it can't be corrected,
  the submission proceeds and the parse is flagged prominently — never left
  silent.
- **Verify on the employer's own board** before entering anything, and drop
  expired/not-found roles without substituting a similar title. Aggregator
  listings are discovery only; a stale one has presented a filled role as
  open more than once.

## 5. Prep-file staleness (the resume wins)

Prep files are built from summaries and go stale. When a prep file and the
resume disagree, the resume wins and the correction is noted in the report.
"No salary figure" in a prep file means *waiting*, not "estimate one."

## 6. Source-field fallback ladder (never invent a discovery channel)

1. "Job board" if offered. 2. "Other" if offered. 3. If neither exists, the
only option that doesn't fabricate a channel — e.g. "applied directly
through the company's website," which is literally true — flagged
prominently. Never select a named aggregator, recruiter, or employee
referral unless actually true.

## 7. Everything flagged in the run report

Estimated salaries; fallback source selections; travel answers; self-ID
declines; prior-application non-disclosures (where a form asked about
employment only, so no false claim was made); auto-parsed fields that
couldn't be corrected; no-cover-letter submissions; unconfirmed location
eligibility; AI-use checkbox selections; every prep-file correction applied; every
verification-pass demotion; roles with no posted range; standing answers
used. The report is the audit surface: one message per run, follow-ups
first, then the tally by band, then the flags.

## 8. Standing answers (the mechanism)

Routine screening questions recur on nearly every form — the same identity,
eligibility, and consent categories, asked a dozen different ways. The
operator answered each category
once, in advance, in writing. The pipeline answers from that table, in
substance verbatim, and the report records which standing answers were used
in each run.

The boundary is strict: a materially different question (a named event, a
date, a specific circumstance), or any category not in the table, holds that
single item for the human. The table is never extrapolated.

Effect, measured: an application held for two days over an 11-item legal
questionnaire was submitted the same afternoon the table was confirmed —
and a whole class of stalls disappeared from the held queue.

## 9. Throughput and run shape

- First run evidence: 33 submitted in ~3 hours with parallel batches of ~8
  browser workers. The per-run cap (20) lives in the run instructions, not
  here — this section is evidence about pace, not authorization to exceed it.
- Each run opens with a follow-up scan: employer replies since the last run
  update the tracker, outcomes are recorded against the band that produced
  them, and anything needing the operator's voice becomes a "Waiting on you"
  item. The pipeline never replies to an employer itself.
- Carry-over before discovery: gate-passing roles found but not attempted in
  the previous run are re-verified and worked first, then the full sourcing
  sweep runs. Dedupe excludes only what was already applied to or seen —
  never the hunt itself.
- Every outcome is logged as it lands (submitted with confirmation text /
  held with reason / washed / dropped / gate-skipped with the gate named /
  weak-fit with missing requirements named). The run report is built from
  that log, never from memory.
- Before any submission, the resume source is re-checked for updates; if it
  changed, per-role resume variants are rebuilt from the fresh record.
