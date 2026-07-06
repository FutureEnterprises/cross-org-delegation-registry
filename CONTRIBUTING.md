# Contributing

## Adding or correcting a row

1. One row per mechanism per claim. Follow
   [`schema/row-schema.md`](schema/row-schema.md).
2. You may author rows only for a mechanism you supply. Do not rewrite
   another supplier's row; propose corrections to them, or on the WIMSE
   list.
3. State the claim precisely, including what it does **not** claim. A carrier
   does not imply a claim; a claim does not imply a verifier; an inherited
   mechanism is not a guarantee unless its dependency and failure behavior
   are stated.
4. Cite public tests and vectors in the `evidence` field. If none exist yet,
   say so explicitly. An empty evidence cell is allowed to say so; a hidden
   empty cell is not.
5. Name the exact draft revisions you evaluated against in
   `evaluated_against`. Verdicts are pinned to those revisions.
6. Declare `gradient_position`, `c_id`, and `r_mapping`. If a C-ID to R
   mapping is not yet reconciled on the list, mark it proposed.

## No overclaiming

Rows are evidence, not marketing. Do not assert a security property the
cited vectors do not demonstrate. Prefer "not yet implemented" or "open"
over an aspirational claim. Reviewers will hold rows to the matrix rules
above, and a row that overclaims will be sent back.

## Anchors and reconciliation

- Requirements (R1-R9) are Morgan Reece's, in
  `draft-reece-wimse-cross-org-delegation`.
- Claims (C-IDs) are Songbo Bu's, in
  `draft-bu-agentproto-security-principal-binding` Section 9.
- The C-ID to R crosswalk and the `gradient_position` shape are reconciled
  on the IETF WIMSE mailing list with both authors present, not settled in
  this repo. This repo records the outcome; the list is the venue.
