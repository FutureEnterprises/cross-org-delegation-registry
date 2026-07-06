# Registry rows

One row per mechanism per claim. Each row follows
[`../schema/row-schema.md`](../schema/row-schema.md). Suppliers add and
correct their own rows by pull request; see
[`../CONTRIBUTING.md`](../CONTRIBUTING.md).

Rows below the seed are contributed by their suppliers. A `TODO` row is a
placeholder inviting the named supplier's PR; it asserts nothing until they
fill it.

---

## Seed row (worked example)

### PEDIGREE / C-002 (authority)

- **c_id:** C-002 (who authorized the delegation)
- **r_mapping:** R5, R1
- **mechanism:** PEDIGREE per-hop delegation chain
- **supplier:** Glyphzero, Inc.
- **claim:** authority for this operation was conveyed from the root
  principal and narrowed at every hop; the terminal scope covers the
  operation. Does not by itself prove holder possession (R4), human
  authorization, or acceptance.
- **carrier:** per-hop delegation tokens, conveyed inline with the request.
- **verifier_and_rule:** relying party, offline: re-verify each hop's
  signature against its issuer key, check per-hop scope subsetting and
  mandate narrowing, take effective expiry as the minimum over hops,
  require the operator-ceiling conjunct.
- **binding_and_freshness:** bound to the root principal and the holder's
  key; staleness bounded by chain lifetime. Fail-closed on stale revocation
  data is NOT yet normative in PEDIGREE (see the C-008 row); this is an
  admitted gap, scheduled for pedigree-02.
- **failure_behavior:** any hop signature failure, subset violation, or
  expiry fails the request as an authorization failure, independent of
  possession and of human authorization.
- **dependency:** the originating organization's trust anchor (root only;
  self-certifying identifiers cover intermediate hops), inline conveyance of
  parent tokens, and a possession-proving transport for holder proof.
- **evidence:** public test vectors NOT yet published. This cell is open and
  says so.
- **gradient_position:** `{ "root": "general_infrastructure",
  "consequence_tier": "software", "sufficiency_bar": "none" }` (root anchor
  acquisition; a continuity mechanism to amortize the root pin across
  rotation is a PEDIGREE provisioning item, not yet specified).
- **spec_status:** specified (chain verification); planned (normative
  fail-closed revocation, pedigree-02).
- **impl_status:** partial.
- **evaluated_against:** draft-rampalli-pedigree-01; reece-00; draft-bu-02.

---

## Contributed rows

_PR your rows here. Placeholders below name the expected supplier and claim;
they assert nothing until filled by that supplier._

- **TODO EMILIA Protocol / C-005, C-007, C-008, C-011, C-012** - EP
  carriers (admissibility profiles, effect_attestation and ceremony_evidence
  AEG nodes, EP-WITNESS-v1 cosignatures, RFC-3161 timestamp), with tests and
  vectors, referencing receipts-06, quorum-02, evidence-record-01,
  evidence-chain-02. Source: EP-CID-R1R9-CROSSWALK.md.
- **TODO WinMagic / possession row** - Live Key condition-bound credential
  (possession under condition), reconciled Nguyen-Huu and Bu row. Source:
  WIMSE condition-bounded thread.
