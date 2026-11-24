# Gate on facts: rebuilding an agent pipeline that graded on vibes

*How my job-application agent's scoring fell apart under inspection, what
replaced it, and what I'd carry into any team putting agents to work on real
systems.*

---

For the last two weeks, an agent system I run on my own infrastructure has
been applying to jobs for me. In its first eleven days: **87
applications submitted, every one verified against an employer's
confirmation page and an email receipt.** The first run alone put
in 33 applications in about three hours.

The volume is the least interesting part. The interesting part is the
morning I asked a simple question — *how do you actually score these?* — and
the system fell apart.

## The score was a vibe

Version one scored each posting 0–100 for fit and submitted anything over
80. It felt rigorous. It was not. When I pulled the run logs and compared
readers, the same role had been scored **82 by one pass and 76 by another,
in the same run**. Another swung 86 to 74. There were no weights, no
definitions, no anchors — the number was a summary of a mood.

An 80+ threshold sitting inside a ±6-point noise band doesn't select for
fit. It selects for which reader happened to pick up the posting. I was
gating real decisions — my name, on real applications — on noise.

So the scoring got rebuilt, on one principle:

> **Gate on facts, derive the number from evidence, and let outcomes
> calibrate it over time.**

## What replaced it

**Hard gates are facts, and any failure ends the role.** Is the posting
actually live on the employer's own careers board (not just an aggregator —
aggregators had already presented me with one filled role as open)? Does the
posted salary range clear a floor? Is the location workable? Is the role's
core duty managing people, which I don't do? Does it require, as a core
requirement, something I factually don't have — a certification, a regulated
domain, a technical depth my resume doesn't show? Judgment calls don't live
here. If a requirement is listed as "preferred," it is by definition not a
gate.

**Fit is a checklist with citations.** For each surviving posting, the
system extracts the must-haves — at most eight — and marks each one
Evidenced, Partial, or Missing. Evidenced requires an exact citation to a
resume bullet. No citation, no credit. This is the audit trail: if it isn't
in the prep file, it didn't happen.

**The number is computed, not estimated.** Four anchored dimensions —
coverage of the checklist (weighted 40%), seniority match, domain adjacency,
scope match — combine into a composite. Strong is 75+, Workable is 55–74,
below that isn't submitted and the report names the missing requirements
instead. Re-reading the same posting against the same resume must reproduce
the same number, or the rubric is broken.

**Verification can only demote.** Before submission, a second pass re-checks
the checklist citations — only the citations. If one doesn't hold, the item
drops, the score is recomputed, and the demotion is logged. The second
reader may never re-score upward. That asymmetry is the whole point: the
original failure was a second reader silently re-rolling the number, so the
fix forbids exactly that and nothing else.

## Most "human in the loop" stalls are unwritten policy

The rebuild's second lesson came from the held queue. One application had
sat blocked for two days because the form included an 11-item legal
questionnaire. The agent was right not to guess at legal attestations. But
the questions were routine — the same categories that appear on form after
form.

So I answered each category once, in advance, in writing: a standing-answers
table. The pipeline now answers routine screening from the table and flags
which answers it used; anything materially different, or outside the table,
still holds that single item for me. The table is never extrapolated.

That application went in the same afternoon. And it generalizes well
past job applications: when an agent keeps stopping to ask you the same
class of question, the fix usually isn't a smarter agent. It's writing the
policy down.

## Verify outcomes, not intentions

Two operational rules earned their place the hard way:

- A browser tab froze after a Submit click, showing no confirmation. The
  employer's thank-you email was already in my inbox. Rule, ever since: **a
  frozen tab doesn't mean failure — check the mailbox before re-entering
  anything.** A duplicate application is worse than a slow one.
- One major platform's interactive verification rejects even correctly
  completed challenges from an automated browser. Three fully prepared
  applications were lost to it in a single run. They're logged as a platform limit, with
  prep work preserved for manual submission — not retried forever, and not
  counted as submitted. Submission means a confirmation page *and* exactly
  one receipt. Nothing else counts.

## The honest scoreboard

Ten outcomes have come back against its first 87 submissions (which ran
under the pipeline's earlier scoring system — the rubric was rebuilt on the
last day of that window, and their bands were assigned retrospectively):
nine employer
decisions, all passes or filled roles, and one application I closed myself
after the employer went quiet. I'm not going to dress that up: the
conversion data is young, and mostly what it says so far is that the top of
the market is competitive.

But here's the thing the old system could never have told me: because every
submission carries its band and composite in the tracker, and every outcome
is recorded against them, I can actually learn from those ten. A monthly
review compares conversion by band. If Workable converts as well as Strong,
the bands move. If Strong isn't converting, the checklist extraction is too
generous and gets tightened — with my sign-off, in writing, in the rubric's
change log. The system isn't just applying. It's instrumented to find out
whether its own judgments are any good.

## What I'd carry into a team

This was a job search, but the shape of the problem is the shape of every
agent deployment I've seen: consequential actions, flaky external systems,
and a model that will happily improvise the difference. Five things
transferred:

1. **Gate on facts.** Separate the checks that are objectively true from the
   judgments that aren't, and let only the facts end a run.
2. **Derive numbers from cited evidence.** If a score can't be traced to an
   artifact, it's a vibe with decimals.
3. **Make verification asymmetric.** A second pass that can only demote is
   worth more than a second opinion that can re-roll everything.
4. **Write standing authority down.** Pre-answered policy, dated and scoped,
   converts recurring stalls into throughput — and stays revocable.
5. **Log outcomes against decisions.** An agent system without calibration
   data is just automation. With it, it's an operation that gets better on
   purpose.

The full methodology — rubric, playbook, architecture, and the run rules —
is in this repo. The applications are still going out every afternoon.

---

*Stuart Rutty is a delivery and technical program manager in Brooklyn, NY,
most recently running service delivery at a hyperautomation consultancy. He
builds and operates agent systems on his own infrastructure.*
