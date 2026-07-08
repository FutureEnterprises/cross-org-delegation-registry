# Row schema

Every row is one mechanism making one claim. Each row declares the columns
below. The first block follows the mapping-table template of
`draft-bu-agentproto-security-principal-binding` Section 14; `c_id`,
`r_mapping`, and `gradient_position` are the registry's additions.

| Field | Meaning |
|-------|---------|
| `c_id` | The claim this row asserts, from [`claims/c-ids.md`](../claims/c-ids.md). |
| `r_mapping` | The requirement(s) this claim answers, from [`requirements/r1-r9.md`](../requirements/r1-r9.md). |
| `mechanism` | The mechanism or carrier making the claim. |
| `supplier` | The party who authored and maintains this row. |
| `claim` | Precisely what is asserted, and what is not. |
| `carrier` | The field, credential, token, or reference carrying the claim. |
| `verifier_and_rule` | Who checks it, and the rule applied. |
| `binding_and_freshness` | What the claim is bound to; replay, revocation, expiry. |
| `failure_behavior` | Behavior when the claim is absent, stale, or unverifiable. |
| `accepted_result` | The constrained verifier result that may be consumed after successful verification, and what that result does not authorize. Required on every row, whatever its `c_id`; C-011 remains a claim class, this field is the row-level statement. |
| `dependency` | External document, service, channel, or arrangement relied on. |
| `evidence` | Public tests and vectors, or an explicit statement that none exist yet. Must declare `evidence_type`. |
| `evidence_type` | Mandatory inside `evidence`. One of: `local_harness`, `cross_language_consistency`, `independent_interop`, `external_implementation`, `none`. Distinct signals do not collapse: a local harness is not interop, and empty evidence says `none`. |
| `gradient_position` | Where the mechanism sits on the first-contact trust gradient. See [`../gradient.md`](../gradient.md). |
| `spec_status` | specified / planned / inherited / assumption. |
| `impl_status` | implemented / partial / none / external. |
| `evaluated_against` | The exact draft revision each cited requirement and claim was read from. |

## Matrix rules (from draft-bu Section 10)

- A carrier does not imply a claim unless the claim is explicitly stated.
- A claim does not imply a verifier unless the verifier is identified.
- An inherited mechanism is not a current guarantee unless its dependency
  and failure behavior are stated.

A row that cannot satisfy these rules is incomplete, not a pass.
