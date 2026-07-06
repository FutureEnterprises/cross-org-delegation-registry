# Cross-Organization Agent Delegation: Conformance Registry

A living, community-maintained registry of how agent-delegation mechanisms
map against a shared set of requirements and a shared claim taxonomy. It is
the running counterpart to the point-in-time Internet-Draft
`draft-rampalli-cross-org-delegation-mapping`.

This registry is editorially neutral. It records how each mechanism's
suppliers describe their own rows, evaluated against two independently
authored anchors. It does not endorse or rank mechanisms.

## Two anchors, two axes

The registry is organized around two orthogonal axes, joined per row. They
are co-equal; neither is subordinate to the other.

- **Requirements (R1-R9)** answer "what must a mechanism provide." Authored
  by Morgan Reece in `draft-reece-wimse-cross-org-delegation`. See
  [`requirements/r1-r9.md`](requirements/r1-r9.md).
- **Claims (C-IDs)** answer "what is this row asserting." Authored by Songbo
  Bu in `draft-bu-agentproto-security-principal-binding`, Section 9. See
  [`claims/c-ids.md`](claims/c-ids.md).

A single row (one mechanism making one claim) names both: the C-ID it
asserts and the R requirement(s) that claim answers. The mapping is
many-to-many.

## What is in here

- [`requirements/r1-r9.md`](requirements/r1-r9.md) - the R1-R9 anchor
  (glossed here, authoritative text in reece-00).
- [`claims/c-ids.md`](claims/c-ids.md) - the C-ID claim taxonomy (criterion
  question per C-ID, authoritative text in draft-bu Section 9).
- [`schema/row-schema.md`](schema/row-schema.md) - the columns every row
  declares, including the `gradient_position` field.
- [`registry/rows.md`](registry/rows.md) - the rows themselves. Suppliers
  contribute their own rows by pull request.
- [`gradient.md`](gradient.md) - the `gradient_position` field definition
  (proposed; pending ratification).

## Governance

- **Suppliers hold their rows.** A mechanism's row is authored and corrected
  by that mechanism's supplier. The editor does not rewrite a supplier's
  claims.
- **Corrections happen in the open.** The canonical venue for corrections is
  the IETF WIMSE mailing list. Registry entries cite the specific draft
  revision they were evaluated against; a verdict does not carry forward to
  a later revision automatically.
- **Anchors are cited, not reproduced.** The authoritative requirement and
  claim text lives in reece-00 and draft-bu respectively. This registry
  glosses and references them; it does not restate their normative text.
- **Evidence, not assertion.** A row that claims a property should cite the
  public tests and vectors that demonstrate it. An empty evidence cell is
  allowed to say so.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) to add or correct a row.

## Status

Seeded 2026-07. Individual community effort, not an IETF Working Group
product. Referenced drafts are individual Internet-Drafts with no WG
adoption at the time of seeding.
