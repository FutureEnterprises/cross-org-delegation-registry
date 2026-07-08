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
- **accepted_result:** on success the verifier may consume exactly: "this
  chain conveys the stated terminal scope for this operation from the root
  principal, at verification time." It does not authorize consuming holder
  possession, human authorization, execution evidence, physical completion,
  or relying-party policy acceptance.
- **dependency:** the originating organization's trust anchor (root only;
  self-certifying identifiers cover intermediate hops), inline conveyance of
  parent tokens, and a possession-proving transport for holder proof.
- **evidence:** public test vectors NOT yet published. This cell is open and
  says so. `evidence_type: none`.
- **gradient_position:** `{ "root": "general_infrastructure",
  "consequence_tier": "software", "sufficiency_bar": "none" }` (root anchor
  acquisition; a continuity mechanism to amortize the root pin across
  rotation is a PEDIGREE provisioning item, not yet specified).
- **spec_status:** specified (chain verification); planned (normative
  fail-closed revocation, pedigree-02).
- **impl_status:** partial.
- **evaluated_against:** draft-rampalli-pedigree-00 (the only revision
  posted; -01 and -02 are in the submission pipeline, and this row
  re-evaluates when they post); reece-00; draft-bu-02.

---

## Contributed rows

_PR your rows here. Placeholders below name the expected supplier and claim;
they assert nothing until filled by that supplier._

### EMILIA Protocol / C-005 (action evidence)

- **c_id:** C-005 (what action was requested, attempted, completed, blocked, or failed)
- **r_mapping:** R8 (primary), R6
- **mechanism:** EP-AEG-v1 action evidence graph + tamper-evident gate evidence log + signed EP-RELIANCE-RESULT-v1, carrying `ceremony_evidence` and `effect_attestation` nodes.
- **supplier:** EMILIA Protocol, Inc.
- **claim:** a content-addressed graph of references to heterogeneous signed artifacts records the action and the evidence attending it; edges are presenter claims verified against artifact bytes, so a lying edge poisons the verdict. It records what action was taken and what evidence supports it. It does NOT by itself establish authority (C-002), provenance truth (C-007), physical completion, independent observation, or relying-party acceptance (C-011).
- **carrier:** AEG nodes and edges bound by a shared action digest; hash-chained gate evidence-log entries; a signed EP-RELIANCE-RESULT-v1 verdict.
- **verifier_and_rule:** relying party, offline: re-verify each referenced artifact's signature against its own issuer key, recompute the graph's content addresses, and replay a relying-party-authored EP-ADMISSIBILITY-PROFILE (content-addressed by `profile_hash`) to a closed five-state verdict (`admissible | missing_evidence | stale | conflicted | unverifiable`). EMILIA authors no bar and is never in the trust path.
- **binding_and_freshness:** every node bound to the action digest; the verdict is recomputable from the pinned profile (deterministic replay digest); freshness is the profile's staleness input, graded by the relying party per consequence.
- **failure_behavior:** a missing, unverifiable, or mis-bound artifact yields a non-admissible verdict (`missing_evidence` / `unverifiable` / `conflicted`), never a pass. Fail-closed.
- **accepted_result:** on success the relying party may consume exactly: "this evidence graph records the stated action under artifacts that verify, admissible under profile `<profile_hash>` at verification time." It does not authorize consuming authority, provenance truth, physical completion, independent verification, or any acceptance beyond the pinned profile.
- **dependency:** the relying party's own pinned EP-ADMISSIBILITY-PROFILE; the issuer key of each referenced artifact; a transport that preserves the artifact bytes.
- **evidence:** `tests/evidence-graph.test.js` and `tests/admissibility-profiles.test.js` exercise the AEG and admissibility-replay carrier in-repo; the adjacent `evidence-record.v1.json` (5) and `provenance.exec.v1.json` (6) suites run under `node conformance/run.mjs`. `evidence_type: local_harness` — the AEG and admissibility mechanism is covered by EP's own in-repo JavaScript tests, not by a cross-language runner; the evidence-record and provenance suites that ARE cross-language are stated on the C-007 row rather than claimed here.
- **gradient_position:** `{ "root": "pinned_root", "consequence_tier": "software", "sufficiency_bar": "<profile_hash>" }` — the sufficiency bar is the relying-party-pinned admissibility profile; the consequence tier rises with the profile's required assurance.
- **spec_status:** specified (AEG, admissibility replay).
- **impl_status:** implemented.
- **evaluated_against:** evidence-chain-02; reece-00 (R8, R6); draft-bu-02 (C-005).

### EMILIA Protocol / C-007 (evidence provenance)

- **c_id:** C-007 (what evidence, signature, receipt, attestation, log entry, or record supports an action or decision)
- **r_mapping:** R8
- **mechanism:** EP provenance chains and long-term evidence records (RFC 4998-style renewal across algorithm aging), with optional EP-WITNESS-v1 cosignatures and optional RFC-3161 timestamp proofs.
- **supplier:** EMILIA Protocol, Inc.
- **claim:** each supporting artifact is checkable on its own leg and reports a separate named result; provenance is composed by reference, joined to the action by digest, never collapsed into one boolean. It shows what evidence supports the action. It does NOT assert the authority behind that evidence (C-002), current validity (C-008), or acceptance (C-011).
- **carrier:** signed provenance-chain links; EP-EVIDENCE-RECORD-v1 renewal chains; EP-WITNESS-v1 cosignatures over a checkpoint head; RFC-3161 timestamp tokens.
- **verifier_and_rule:** relying party, offline: verify each leg's Ed25519 signature over JCS-canonical bytes; a transparency inclusion proof is only as strong as the log operator's checkpoint, raised toward "established" by k distinct pinned witness cosignatures; a timestamp token is verified against a pinned TSA key.
- **binding_and_freshness:** every leg bound to the same action digest; a witness quorum proves k trusted witnesses attested to ONE head (local single-view), not the absence of a different head shown elsewhere; a timestamp proves existence-at-time, not correctness.
- **failure_behavior:** a missing, malformed, or mis-bound leg fails that leg's named check; no leg's failure is masked by another's pass. Fail-closed per leg.
- **accepted_result:** on success the relying party may consume exactly: "these named legs (provenance / evidence-record / witness / timestamp) each verified under their pinned roots at verification time." It does not authorize consuming any leg as authority, as current validity, or as relying-party acceptance.
- **dependency:** the pinned issuer keys per leg; a pinned log checkpoint key and pinned witness keys for inclusion strength; a pinned TSA key for timestamps.
- **evidence:** `provenance.exec.v1.json` (6), `evidence-record.v1.json` (5), `witness.v1.json` (6), and `timestamp-proof.v1.json` (13), all run under `node conformance/run.mjs`. `evidence_type: cross_language_consistency` — EP's JavaScript, Python, and Go verifiers agree on these suites; this is one team in one repository, a consistency check, not independent reimplementation. (An externally authored from-spec Rust verifier, public source, is reported on the list to agree on the full published vector set; that external agreement is auditable in its source and is not claimed here as the row's `evidence_type`.)
- **gradient_position:** `{ "root": "general_infrastructure", "consequence_tier": "software", "sufficiency_bar": "none" }` — native signatures root in general infrastructure; transparency and witness strength are relying-party pins graded per consequence.
- **spec_status:** specified.
- **impl_status:** implemented.
- **evaluated_against:** evidence-record-01; reece-00 (R8); draft-bu-02 (C-007).

### EMILIA Protocol / C-008 (freshness or revocation)

- **c_id:** C-008 (is the authority, delegation, instance state, tool binding, or session state still current)
- **r_mapping:** R7
- **mechanism:** validity windows; a portable, offline-verifiable EP revocation statement; signed time-attestation including an RFC-3161 timestamp proof against a pinned TSA key.
- **supplier:** EMILIA Protocol, Inc.
- **claim:** a revocation statement is an authentic, portable, offline-checkable claim that a previously valid authorization is now revoked, binding the exact `(target_type, target_id, action_hash)`. It answers whether the material is current. It does NOT prove you hold the latest revocation state (absence of a statement is not proof of not-revoked; that is a distribution problem left to the relying party).
- **carrier:** an Ed25519-signed EP-REVOCATION-v1 statement; validity-window fields; an EP-TIME-ATTESTATION-v1 / RFC-3161 token.
- **verifier_and_rule:** relying party, offline: accept a revocation only under a key pinned for the revoker, binding the exact target and action hash; evaluate the validity window; verify the timestamp against a pinned TSA key. The staleness bound is a relying-party policy input graded per consequence.
- **binding_and_freshness:** the statement binds the exact target and action hash (revoking A never revokes B); the freshness bound is the relying party's, not the issuer's; stale material fails safe, never open.
- **failure_behavior:** an unpinned revoker key, a target mismatch, a malformed timestamp, or material older than the pinned staleness bound fails closed; the authorization is treated as not current.
- **accepted_result:** on success the relying party may consume exactly: "this authorization is (not) revoked as of an authentic statement binding this exact target, within the relying party's staleness bound." It does not authorize treating absence of a statement as proof of currency, nor consuming the result as authority.
- **dependency:** the pinned revoker key; the pinned TSA key; the relying party's own revocation distribution channel and staleness policy.
- **evidence:** `revocation.exec.v1.json` (6), `time-attestation.v1.json` (6), and `timestamp-proof.v1.json` (13), all run under `node conformance/run.mjs`. `evidence_type: cross_language_consistency` — EP's JavaScript, Python, and Go verifiers agree; one team, one repository, a consistency check, not independent reimplementation.
- **gradient_position:** `{ "root": "general_infrastructure", "consequence_tier": "software", "sufficiency_bar": "none" }` — revocation authenticity is general-infrastructure; the staleness bound is a relying-party pin graded per consequence.
- **spec_status:** specified.
- **impl_status:** implemented.
- **evaluated_against:** evidence-record-01 (time-attestation / timestamp lineage); reece-00 (R7); draft-bu-02 (C-008).

### EMILIA Protocol / C-011 (accepted result)

- **c_id:** C-011 (what normalized result may the application consume after successful verification, and what does that result not authorize)
- **r_mapping:** R2
- **mechanism:** constrained verifier outputs that keep VERIFIED and ACCEPTED separate: `{ valid, checks{...} }` for receipts; `{ verified, accepted, checks }` for pinned-root artifacts; a signed EP-RELIANCE-RESULT-v1; a pinned EP-ADMISSIBILITY-PROFILE closed five-state verdict.
- **supplier:** EMILIA Protocol, Inc.
- **claim:** the verifier returns VERIFIED (cryptographic checks pass, general infrastructure) and ACCEPTED (trusted under a pinned root) as separate fields, never one collapsed boolean, and the result names its own non-claims. This IS the accept side of the trust gradient made explicit. A passing raw claim is not a passing acceptance.
- **carrier:** the verifier result object's separate `verified` / `accepted` (or `valid` / pinned-acceptance) fields; the admissibility verdict plus its deterministic replay digest.
- **verifier_and_rule:** relying party: `verified` is unconditional and offline; `accepted` requires a pre-pinned root (issuer key, approver directory, or admissibility `profile_hash`). A raw claim that verifies but is not pinned is reported verified-and-not-accepted, never passed through as accepted.
- **binding_and_freshness:** acceptance is bound to the relying party's pinned root at evaluation time; the admissibility verdict is recomputable via its replay digest, so the acceptance decision is auditable, not asserted.
- **failure_behavior:** a raw claim presented for pass-through, an unpinned root, or a profile mismatch yields `accepted: false` (or a non-admissible verdict) while `verified` is reported honestly; the two are never merged.
- **accepted_result:** on success the relying party may consume exactly the constrained result the row's verifier returns (e.g. `verified:true, accepted:true` under pinned root, or an `admissible` verdict under `<profile_hash>`), and nothing the result does not name. It does not authorize consuming `verified` as `accepted`, nor acceptance beyond the pinned root.
- **dependency:** the relying party's pinned root (issuer key / approver directory / admissibility profile hash).
- **evidence:** `boundary.v1.json` case `raw_claim_pass_through` (a four-vector suite run under `node conformance/run.mjs`, JavaScript / Python / Go agree); `examples/binding/human-authorization-binding-vector.mjs` case B3 (verified-but-never-accepted). `evidence_type: cross_language_consistency` — the boundary suite is checked by EP's JS/Python/Go verifiers, one team and one repository, a consistency check; the binding example is an in-repo JavaScript worked case supporting it.
- **gradient_position:** `{ "root": "pinned_root", "consequence_tier": "software", "sufficiency_bar": "<profile_hash | none>" }` — this row IS the gradient's accept side: acceptance requires a pinned root, reported separately from verification.
- **spec_status:** specified.
- **impl_status:** implemented.
- **evaluated_against:** receipts-06, evidence-chain-02; reece-00 (R2); draft-bu-02 (C-011).

### EMILIA Protocol / C-012 (authorization and attribution boundary)

- **c_id:** C-012 (is the row claiming pre-execution authority, delegated scope, post-execution attribution, execution evidence, audit enforcement, or relying-party acceptance, and which does it not claim)
- **r_mapping:** R5 (pre-execution WHO leg), R8 (post-execution attribution leg)
- **mechanism:** pre-execution: EP-RECEIPT-v1 / EP-QUORUM per-action human authorization. Post-execution: EP-AEG-v1 + gate evidence log + EP-EXECUTION-INTEGRITY-v1 (the executor is identified, never trusted), with an `effect_attestation` observed-effect leg.
- **supplier:** EMILIA Protocol, Inc.
- **claim:** the pre-execution authority leg and the post-execution attribution leg sit at different gradient positions and stay there; the shared action digest joins them without making them interchangeable. It states which of pre-execution authority / delegated scope / post-execution attribution / execution evidence / audit enforcement / acceptance a row claims and which it does not. Post-execution attribution roots only in the executor's own pinned key and grants NO authority.
- **carrier:** the receipt / quorum artifact for the WHO leg; the AEG attribution node and execution-integrity record for the post-execution leg; the shared action digest as the join.
- **verifier_and_rule:** relying party, offline: verify the pre-execution WHO leg under its pinned root (issuer / approver directory); verify the post-execution attribution leg only under the executor's own pinned key; confirm both bind the same action digest. Attribution presented as authorization is rejected.
- **binding_and_freshness:** both legs bind the same action digest; neither leg's acceptance transfers to the other; the executor key attributes an effect, it does not authorize it.
- **failure_behavior:** presenting a post-execution attribution (or an execution report) as pre-execution authority is refused; a leg whose digest does not match is rejected. Fail-closed.
- **accepted_result:** on success the relying party may consume exactly the leg it verified, labelled as that leg: "a named human authorized this action" (WHO leg) OR "this executor attributes this effect to this action" (attribution leg), joined by digest, never merged. It does not authorize consuming attribution as authority or an execution report as physical completion.
- **dependency:** the pinned root for the WHO leg; the executor's own pinned key for the attribution leg.
- **evidence:** `boundary.v1.json` case `attribution_substituted_for_authorization` (four-vector suite run under `node conformance/run.mjs`, JavaScript / Python / Go agree); `examples/scitt/capsule-seam-vector.mjs` reject cases (capsule verifier checks the capsule; EP verifier checks the receipt offline; joined only by the shared action digest). `evidence_type: cross_language_consistency` — the boundary suite is checked by EP's JS/Python/Go verifiers, one team, one repository; the seam vector is an in-repo JavaScript worked case supporting it.
- **gradient_position:** `{ "root": "pinned_root", "consequence_tier": "quorum", "sufficiency_bar": "none" }` — the pre-execution authority leg can carry up to M-of-N human quorum; the post-execution leg roots only in the executor's pinned key and grants no authority.
- **spec_status:** specified.
- **impl_status:** implemented.
- **evaluated_against:** receipts-06, quorum-02, evidence-chain-02; reece-00 (R5, R8); draft-bu-02 (C-012).
- **TODO WinMagic / possession row** - Live Key condition-bound credential
  (possession under condition), reconciled Nguyen-Huu and Bu row. Source:
  WIMSE condition-bounded thread.
