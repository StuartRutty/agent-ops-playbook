# Rubric Methods: Evidence-Anchored Job Selection

The formal methods write-up behind [docs/selection-rubric.md](selection-rubric.md):
the five stages in full, with the dimension anchors, the composite formula,
and the calibration rules. The rubric doc is the summary; this is the audit
grade version.

Every role evaluated by the job-application pipeline passes through a deterministic, five-stage selection rubric before an application is prepared. 

Earlier iterations of the pipeline used a direct 0–100 Holistic Fit score with an 80-point cutoff. Audit logs showed that unanchored holistic scoring had a 6-to-12-point variance on identical job descriptions across consecutive passes — meaning a role near the threshold was accepted or rejected on sampling noise rather than qualification fit. On November 13, 2026, the holistic score was retired and replaced with the evidence-anchored method documented below.

The governing rule of this rubric is straightforward: **gate on binary facts, compute fit strictly from cited resume evidence, and restrict verification to demoting unsupported claims.**

---

## Stage 1: Hard Factual Gates

Before any scoring occurs, the pipeline checks six binary preconditions against the live posting. A failure on any single gate ends evaluation immediately, and the exact disqualifying fact is logged verbatim.

| Gate | Disqualification Condition | Boundary & Exception Rules |
| :--- | :--- | :--- |
| **1. Posting Liveness** | No active listing exists on the employer's primary careers board. | Aggregator-only listings (e.g., reposts without an upstream employer URL) fail automatically. |
| **2. Compensation Floor** | The posted base salary minimum is below $100,000. | Listings that omit a salary range are **not** gated out; they remain eligible and are flagged as unstated in the run report. |
| **3. Location** | The role is neither Remote (US-eligible) nor hybrid/on-site in New York City. | Relocation-required roles outside NYC fail immediately. |
| **4. Management Scope** | The core responsibility is people management (hiring, firing, performance reviews, direct reports). | Target scope is strictly Individual Contributor (IC). The system never applies to people-management roles and never fabricates management history. |
| **5. Hard Requirements** | The job description mandates a core qualification the candidate factually lacks — such as a statutory license/certification (e.g., PMP, clinical license), regulated-industry domain tenure, or specialized engineering depth not present on the resume (e.g., IAM/SSO systems engineering). | Applies only to requirements marked *required* or *core*. Items listed as *preferred* or *nice to have* do not trigger a gate failure; they proceed to Stage 2 and are scored in the checklist. |
| **6. Account & Attestation Rules** | The application portal requires creating a dedicated candidate account, or demands personal legal attestations. | Portal account creation is a standing skip. Personal legal attestations are placed on hold for human completion rather than skipped or auto-signed. |

---

## Stage 2: The Cited Requirement Checklist

Roles that clear all six hard gates move to requirement extraction. The evaluator extracts up to **8 core requirements** from the job description — specifically what the employer designates as required or foundational to the role (preferred qualifications are excluded unless they are structurally central to the day-to-day work, in which case they are explicitly tagged).

Each extracted requirement is graded against the candidate's actual resume and written to the role's preparation file under one of three states:

* **Evidenced (1.0 weight):** The resume directly proves the requirement. The checklist entry must quote or cite the specific role and bullet point on the resume.
* **Partial (0.5 weight):** The resume demonstrates adjacent or partial experience. The entry must cite the existing resume evidence and explicitly name the gap between that evidence and the job description's ask.
* **Missing (0.0 weight):** No factual evidence exists on the resume. If a valid resume citation cannot be produced, the requirement defaults to Missing.

The preparation file serves as the immutable audit trail for the application. No score can be generated without a persisted checklist.

---

## Stage 3: Four Anchored Scoring Dimensions

Rather than estimating a 0–100 score directly, the rubric grades four orthogonal dimensions on an integer scale of **0 to 4**, each tied to explicit anchors:

### 1. Coverage (Weight: 40%)
Derived mathematically from Stage 2. Coverage is the share of extracted core requirements satisfied by the resume, where **Evidenced = 1.0**, **Partial = 0.5**, and **Missing = 0.0**:
* **4:** ≥ 90% of checklist evidenced
* **3:** 75% – 89%
* **2:** 60% – 74%
* **1:** 40% – 59%
* **0:** < 40%

### 2. Seniority (Weight: 20%)
Measures alignment between the role's target level and the candidate's career band:
* **4:** Exact level match
* **3:** One step adjacent in either direction
* **2:** Plausible stretch role
* **1:** Two steps removed from target band
* **0:** Entirely different career band

### 3. Domain (Weight: 20%)
Measures industry and product-environment transferability:
* **4:** Direct domain match
* **3:** Closely adjacent domain (e.g., complex B2B SaaS, developer tools, AI product delivery, or B2B legs of healthtech/fintech)
* **2:** Transferable environment with a concrete, demonstrable bridge
* **1:** Unrelated domain, though learnable on the job
* **0:** Hard domain mismatch where prior regulated or vertical expertise is the job itself

### 4. Scope (Weight: 20%)
Measures how closely the day-to-day duties match core technical delivery, implementation, and program execution:
* **4:** Core duties are delivery, implementation, or technical program execution
* **3:** Primarily delivery/implementation, with minor adjacent responsibilities
* **2:** Roughly half of the role sits outside core scope (e.g., heavy presales, pure spec-writing, or tier-1 support)
* **1:** Majority of duties fall outside core scope
* **0:** Functionally a different profession

---

## Stage 4: Composite Calculation & Action Bands

The four dimension scores are combined into a deterministic 0–100 composite using fixed weights:

```text
Composite = 25 × (0.40 × Coverage + 0.20 × Seniority + 0.20 × Domain + 0.20 × Scope)
```

Because Coverage carries 40% of the weight and is calculated directly from resume citations, re-evaluating the same job description against the same resume yields the same score. Roles are bucketed into three operational bands:

| Band | Composite Range | Operational Policy |
| :--- | :--- | :--- |
| **Strong** | **75 – 100** | Priority submission queue. High requirement coverage alongside strong seniority, domain, and scope alignment. |
| **Workable** | **55 – 74** | Secondary submission queue. Submitted after Strong roles are exhausted, ordered highest-composite-first up to the daily batch cap of 20 roles. |
| **Weak** | **Below 55** | **Never submitted.** Filtered out prior to application; the run report logs the specific Missing checklist items so gaps are visible. |

**Why the cutoff sits at 55:** A composite of 55 corresponds to roughly 60%+ core requirement coverage paired with workable seniority and domain alignment. Submitting below that threshold would misrepresent candidate readiness and waste reviewer time on both sides.

---

## Stage 5: Demote-Only Verification Pass

Before any Strong or Workable role is cleared for submission, an independent verification pass audits the preparation file. 

To prevent a second pass from re-introducing subjective score drift, the verifier has strictly bounded authority:
1. It reads **only** the Stage 2 requirement checklist and the source resume.
2. It verifies that every item marked *Evidenced* or *Partial* points to a real resume bullet that factually supports the claim.
3. **Demote-only rule:** If a citation is stretched, inaccurate, or missing, the verifier demotes the requirement (`Evidenced` → `Partial` → `Missing`), logs the demotion in the run report, and recomputes the Coverage dimension and composite score. The verifier cannot upgrade items or perform free-form re-scoring.

If a demotion pushes a role's recomputed composite below 55, the role drops into the Weak band and is withheld from submission.

---

## Calibration & Governance

The rubric treats band thresholds as testable hypotheses rather than static truths:
* **Outcome tracking:** Every submitted application is logged with its composite score and band alongside downstream funnel outcomes (no response at 30 days, rejection, recruiter screen, or hiring-manager interview).
* **Monthly pipeline review:** Conversion rates are reviewed by band on a monthly cadence. If Workable applications convert to screens at the same rate as Strong applications, the band boundary is recalibrated; if Strong applications under-convert, requirement extraction and citation strictness are tightened.
* **Change control:** Weights, dimension anchors, and band cutoffs cannot be altered mid-run. Any revision requires explicit human sign-off and a dated entry in the rubric changelog.
