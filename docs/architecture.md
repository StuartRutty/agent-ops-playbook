# Architecture — how a run fits together

The pipeline is a scheduled job with a written instruction body, executed by
an agent with access to a browser, a mailbox, a document store, and a set of
homelab services. There is no custom orchestrator: the *documents* are the
orchestration. The rubric decides, the playbook constrains, the logs prove.

## Run lifecycle

```
        ┌──────────────────────────────────────────────────────────┐
        │ 1. FOLLOW-UP SCAN                                        │
        │    mailbox replies since last run → tracker updates,     │
        │    outcomes recorded against bands, "Waiting on you"     │
        │    items extracted (the pipeline never replies itself)   │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 2. HELD-QUEUE RE-CHECK                                   │
        │    applications blocked only by now-resolved rules       │
        │    (e.g. a standing answer added since) are completed;   │
        │    items the operator parked personally stay parked      │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 3. CARRY-OVER, THEN DISCOVERY                            │
        │    previous run's unattempted gate-passers re-verified,  │
        │    then parallel scouts sweep employer boards +          │
        │    aggregators (discovery only)                          │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 4. VERIFY → ASSESS → VERIFY                              │
        │    posting confirmed live on the employer's own board →  │
        │    hard gates → requirement checklist with citations →   │
        │    anchored dimensions → computed composite →            │
        │    verification pass (demote-only)                       │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 5. PREP + VARIANTS                                       │
        │    per-role prep file (cover letter stating gaps         │
        │    plainly, screening answers from the record) and a     │
        │    resume variant that re-orders real bullets to lead    │
        │    with the posting's requirements — nothing added       │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 6. SUBMIT (parallel browser workers, batches of ~8)      │
        │    confirmation page captured; anything with an unknown  │
        │    outcome is checked against the mailbox before any     │
        │    retry                                                 │
        └──────────────┬───────────────────────────────────────────┘
                       ▼
        ┌──────────────────────────────────────────────────────────┐
        │ 7. LOG + REPORT                                          │
        │    every outcome written to the run log as it lands;     │
        │    tracker and seen-postings ledger merged; ONE report   │
        │    message: follow-ups, tally by band, all flags         │
        └──────────────────────────────────────────────────────────┘
```

## Components

- **Scheduler** — a daily job (currently mid-afternoon, deliberately: the
  operator is awake for approval-gated steps like email verification codes).
  Runs used to be overnight; moving them into waking hours removed an entire
  class of "held until morning" stalls.
- **Scouts** — parallel sub-agents that sweep sourcing lanes (employer ATS
  boards, aggregators, keyword searches) and return candidate postings. They
  have no write access to anything that matters; the parent verifies every
  shortlisted posting on the employer's own board.
- **Assessor** — the rubric, applied to a prep file per role. The prep file
  is the unit of audit: checklist, citations, dimension scores, composite,
  band.
- **Variant builder** — a small script that takes the master resume (kept in
  a resume service, re-fetched fresh each run) plus a per-role emphasis spec
  and emits a PDF whose existing bullets are re-ordered so the ones matching
  the posting lead. It cannot add content; an emphasis spec that names a
  bullet that doesn't exist fails the build.
- **Submitters** — parallel browser workers, one application each, working
  from the prep file. They ask the parent when a form asks something the
  prep file doesn't answer; the parent answers from the record or holds the
  item.
- **Mailbox as ground truth** — submission success is defined as: employer
  confirmation page seen AND exactly one receipt email present. Either alone
  is investigated, not assumed.
- **Tracker + logs** — a markdown tracker (one row per application, with band
  and composite) and a per-run submissions log are the system's memory.
  Reports are generated from the logs, never from an agent's recollection.

## Trust boundaries

- **Outward actions are the exception, not the default.** Across the wider
  operation, agents read freely and write narrowly: each workload has a
  written scope of what it may change (the meal planner may add to a meal
  plan and a shopping list — nothing else; the job pipeline may submit
  applications that pass its gates — nothing else).
- **Approval-gated steps stay gated.** Email verification codes are shared
  only through an approval card the operator taps. Account creation, any
  spending, and any novel legal attestation never happen inside a run.
- **Standing authority is written down.** The standing-answers table and the
  standing instruction for interactive verification steps are each dated,
  scoped, and recorded in the playbook. If it isn't written, it isn't
  authorized.
- **Sub-agents inherit the rules, not just the task.** Scouts and submitters
  receive the playbook constraints with their assignment; their outputs are
  treated as untrusted until verified against the employer's board, the
  mailbox, or the resume.

## Known limits (documented, not hidden)

- **Captcha verification on one major platform** rejects even correctly
  solved challenges from an automated browser. Three fully prepared
  applications were lost to it in one run; their prep files and resume
  variants are preserved for manual submission. The system logs these as
  platform limits, not as agent failures to retry forever.
- **Aggregator data is unreliable** — stale postings, wrong locations,
  invented salary ranges. Hence the employer-board verification rule, twice.
- **The rubric's calibration data is young.** Ten decisions against 87
  submissions is not enough to move the bands yet; the monthly review exists
  so that when it is enough, the change is deliberate and signed off, not
  drift.
