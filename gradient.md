# The `gradient_position` field

Records where a mechanism sits on the first-contact trust gradient: how a
relying party comes to trust an originating anchor with no bilateral
arrangement specific to the interaction.

The gradient exists because first-contact trust is irreducible. Two parties
with no shared prior root of any kind cannot bootstrap authenticated trust.
What R2 excludes is a bespoke bilateral arrangement, not reliance on general
infrastructure that each party joined independently. The more consequential
the action, the closer first-contact trust tends toward a prior
relationship. A conforming mechanism documents where on this gradient it
operates rather than claiming to eliminate the residual.

## Proposed shape (pending ratification)

```json
{
  "root": "general_infrastructure | pinned_root",
  "consequence_tier": "software | class_a | quorum",
  "sufficiency_bar": "<profile_hash> | none"
}
```

- `root` - whether acceptance rests on general infrastructure alone
  (verification), or on a root the relying party has pinned (acceptance for
  higher-consequence actions).
- `consequence_tier` - the assurance the mechanism operates at: software,
  Class-A device, or an M-of-N human quorum.
- `sufficiency_bar` - a pinned admissibility-profile hash bounding evidence
  sufficiency, or `none`.

**Status: proposed.** Shape contributed by EMILIA Protocol
(EP-CID-R1R9-CROSSWALK.md). It implements the consequence gradient in
Morgan Reece's R2 refinement and the assurance tiers in Songbo Bu's claim
work. It is offered here as a starting point and is not final until
ratified by both authors on the WIMSE list. Amortization note (Reece,
Schrock): once the first-contact pin is taken, a continuity mechanism can
carry the binding across key rotation, so a repeated relationship sits lower
on the gradient than a first encounter. The pin is paid once, amortized, not
eliminated.
