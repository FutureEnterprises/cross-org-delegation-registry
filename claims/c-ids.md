# Claim taxonomy: C-IDs

The claim axis. Answers "what is this row asserting."

**Authoritative text:** `draft-bu-agentproto-security-principal-binding-02`,
Section 9 (Songbo Bu). The criterion questions below are reproduced for
navigation; the normative definitions live in draft-bu Section 9. The
R-mapping column is the crosswalk, not part of Songbo's text; it records
which requirement(s) each claim answers and is reconciled on the WIMSE list
with the claim author present.

| C-ID | Criterion question | Answers (R) |
|------|--------------------|-------------|
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
