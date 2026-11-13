# Selection rubric — evidence-based fit assessment

This replaces a 0–100 gut score and an "80+ submits" bar. The old method was
shown to be unanchored: no weights, no definitions, no calibration — and the
same role swung 6–12 points between two readers in a single run (82→76 on
one, 86→74 on another). A gate set inside that noise band gates on noise.

The principle: **gate on facts, derive the number from evidence, and let
outcomes calibrate it over time.** Every figure in a run report must be
traceable to the requirement checklist in the role's prep file.

## 1. Hard gates — facts only; any failure ends the role, reason logged verbatim

- **Dead/unverified**: no live posting on the employer's own careers board.
  Aggregator-only finds do not pass. (This rule exists because an aggregator
  listing for a filled role survived to the submission step once; now the
  employer's board is the only source of truth, checked before prep work and
  again before submission.)
- **Below floor**: posted range minimum under the operator's floor (a figure
  set privately, read against the live posting). No posted range is NOT a gate —
  the role stays eligible and is flagged in the report.
- **Location**: not workable as remote-US or home-city based.
- **Management scope**: the role's core duty is managing people (hiring,
  firing, direct reports). The candidate is an individual contributor; the
  pipeline never applies to these and never claims management experience.
- **Hard requirement**: the job description requires, as a core requirement,
  something the candidate factually does not have — a certification, a
  regulated-domain expertise, or a technical depth the resume does not
  evidence. Boundary rule: if the item is listed as "preferred" or
  "nice to have," it is NOT a gate — it belongs in the checklist as Missing.
- **Attestation/account**: the application demands personal legal
  attestations outside the standing-answers table (hold for the human, not a
  skip), or requires creating an account (skip, standing rule).

## 2. Requirement checklist — per role, stored in the prep file

Extract the posting's must-haves (at most 8 — the requirements the posting
itself marks as required or core; do not pad with preferred items, but if a
"preferred" item is clearly central to the role, include it and mark it as
such). For each must-have, record:

- **Evidenced** — the resume proves it; cite the exact bullet or role.
- **Partial** — adjacent or partial evidence; cite what exists and name the gap.
- **Missing** — no evidence. No citation can be invented; an item without a
  real citation is Missing.

This checklist is the audit trail. If it isn't in the prep file, it didn't
happen.

## 3. Dimension scores — 0–4 each, anchored

- **Coverage**: evidenced share of the checklist (Partial counts ½).
  ≥90% → 4 · 75–89% → 3 · 60–74% → 2 · 40–59% → 1 · <40% → 0.
- **Seniority**: exact level match → 4 · one step adjacent (either
  direction) → 3 · plausible stretch → 2 · two steps off → 1 · wrong band
  entirely → 0.
- **Domain**: same domain → 4 · adjacent (complex B2B SaaS delivery ↔
  healthtech/fintech with a B2B leg, devtools, AI product delivery) → 3 ·
  transferable with a real bridge → 2 · unrelated but learnable → 1 · hard
  mismatch (regulated expertise is the job) → 0.
- **Scope**: core duties are delivery / implementation / program work → 4 ·
  mostly that, some adjacent duties → 3 · about half outside (presales,
  spec-writing, support) → 2 · mostly outside → 1 · essentially a different
  job → 0.

## 4. Composite and bands

Composite = 25 × (0.40·Coverage + 0.20·Seniority + 0.20·Domain + 0.20·Scope),
giving 0–100. It is *computed* from §2–§3, never estimated directly —
re-reading the same posting against the same resume must reproduce the same
number.

Bands: **Strong 75–100 · Workable 55–74 · Weak below 55.**

Submission rule: roles that pass every gate are submitted best-first —
Strong, then Workable — until the run cap (20) is reached. Weak roles are
not submitted, and the report names their missing requirements (that list is
the useful output, not the number). Rationale for the Workable line: 55 ≈ at
least ~60% of must-haves evidenced with reasonable seniority/domain fit;
below that, an application would misrepresent fit — a claim about evidence,
not a vibe.

## 5. Verification pass — check evidence, not vibes

Before any submission, a second read checks ONLY the checklist: every
Evidenced or Partial item must cite a resume bullet that actually says that.
A citation that doesn't hold demotes the item (Evidenced → Partial →
Missing), the dimension scores and composite are recomputed, and the
demotion is logged in the run log. No free-form re-scoring — a second reader
silently re-rolling the whole number is exactly the failure this forbids.

## 6. Calibration — the system earns its lines with data

- The tracker records each submission's band and composite alongside the
  application.
- When an outcome lands (no reply after 30 days / rejection / screen /
  interview), the follow-up scan records it against that band.
- A weekly sweep reviews conversion by band roughly monthly. If Workable
  converts as well as Strong, the bands move; if Strong isn't converting,
  the checklist extraction is too generous. Any change to anchors, weights,
  or band lines needs the operator's sign-off and a dated note in this file.

Until that data exists, treat the composite as an ordering device with an
evidence trail — which is already a large improvement over a number from
nowhere.

## Change log

- 2026-11-13: Rubric created; replaces the 0–100 gut score + 80+ gate. Same
  day the batch moved from overnight to afternoon runs (so the operator is
  awake for approval-gated steps) and the per-run cap rose from 12 to 20.
