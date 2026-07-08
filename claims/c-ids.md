# Claim taxonomy: C-IDs

The claim axis. Answers "what is this row asserting."

**Authoritative text:** `draft-bu-agentproto-security-principal-binding-02`,
Section 9 (Songbo Bu). The criterion questions below are reproduced for
navigation; the normative definitions live in draft-bu Section 9. The
"Observed R anchors" column is the crosswalk, not part of Songbo's text: it
records the requirement anchors observed in contributed rows and crosswalks
to date. It is non-exhaustive, and it does not subordinate the C-ID taxonomy
to R1-R9; the two are co-equal axes. It is reconciled on the WIMSE list with
the claim author present.

**Row-level rule (Bu, WIMSE list, 2026-07-07):** a C-ID is not treated as
satisfying an R by itself. The row, with its verifier, failure behavior,
accepted result, and evidence, is what answers a requirement.

| C-ID | Criterion question | Observed R anchors |
|------|--------------------|--------------------|
| C-002 | Who authorized the task, policy, role, or delegation? | R5, R1 |
| C-005 | What action was requested, attempted, completed, blocked, or failed? | R8, R6 |
| C-007 | What evidence, signature, receipt, attestation, log entry, or record supports an action? | R8 |
| C-008 | Is the authority, delegation, instance state, tool binding, or session state still current? | R7 |
| C-011 | What normalized result may the application consume after successful verification, and what does that result not authorize? | R2 |
| C-012 | Is the row claiming pre-execution authority, delegated scope, post-execution attribution, execution evidence, audit enforcement, or acceptance, and which does it not claim? | R5 (pre), R8 (post) |

This table is seeded from the EMILIA Protocol C-ID to R1-R9 crosswalk
(EP-CID-R1R9-CROSSWALK.md) and from draft-bu Section 9. The C-ID to R
reconciliation is not final until confirmed on the WIMSE list with the
claim author (Songbo Bu) and the requirements author (Morgan Reece). Only
the six C-IDs currently referenced by contributed rows are listed; the full
taxonomy is in draft-bu Section 9.
