# From marketplace reports to a community specification

**Proposed Open Markets Foundation research process · v1.0.0 · 2026-11-05**

This guide explains how to combine repository audits into comparable evidence and proposed specification changes. Its thresholds and review steps are proposed research practices, not a claim about existing Open Markets Foundation governance. An agent report, a ranking, or a popular implementation does not make a protocol rule normative.

Produce individual reports using [AUDIT_GUIDE.md](AUDIT_GUIDE.md), [AGENT_PROMPT.md](AGENT_PROMPT.md), [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md), and [report.schema.json](report.schema.json). This guide governs the next step: comparing those reports without losing their evidence, limitations, or disagreements.

## 1. Admit evidence before comparing projects

A report is eligible for synthesis when it identifies the project, canonical repositories where known, audit date, product role, inspected components, and unavailable components. Source-based findings need exact inspected commits or an explicit pinning limitation. Documentary and black-box reports may contribute bounded non-source observations with their own provenance. Their structured records must validate, and consequential findings must resolve to evidence records. Validation establishes structural consistency; a reviewer must still check what the evidence proves. An exact primary source commit remains mandatory for the source-based ranking.

Reports with blocked access or unresolved findings can contribute bounded observations. Mark their limitations explicitly. Do not discard an inconvenient result or promote an incomplete report into a complete comparison.

Maintain a production-evidence facet separately:

- **Unverified:** deployment evidence has not been established.
- **Project claimed:** the project identifies a production deployment.
- **Observed:** the service was observed at a recorded time.
- **Commit linked:** an observed deployment is demonstrably linked to the inspected commit. A release artifact without deployment evidence remains a release observation.

Only observed services with an evidenced **production** environment classification belong in a list labelled **observed production deployments**. Keep observed staging, demo, and unknown environments separate. Claimed, historical, experimental, and unavailable deployments remain useful research sources, identified as such. A reachable website proves neither transactions nor a connection to a particular repository revision. Do not infer users, revenue, security, or commercial maturity from this facet.

## 2. Normalize capabilities while preserving wire variants

Organize findings around four workflow stages. These are comparison categories, not a requirement that each stage have its own event kind.

| Stage | Questions to reconcile |
| --- | --- |
| **Offer** | How are goods or services described, discovered, identified, priced, updated, withdrawn, and referenced? |
| **Order** | How do participants express intent, negotiate, accept, reject, cancel, and track an order? |
| **Payment** | How are payment requests, assets, amounts, settlement, and order associations represented and verified? |
| **Resolution** | How are completion, receipts, disputes, returns, refunds, and other outcomes represented? |

Map each finding to a narrowly defined capability within a stage. Record producer and consumer behavior separately. Preserve its original report ID and evidence IDs so a normalized summary remains traceable.

Do not merge variants merely because they share a kind or NIP name. Retain differences in tags, payload fields, requiredness, identifiers, references, amount units, currencies, encryption envelopes, recipients, replacement rules, deletion, transition ordering, relay queries, and trust assumptions. Distinguish a missing field from an optional field and an unsupported operation from an unexamined operation.

Create one variant record per materially different wire behavior. Give each variant a stable identifier and describe whether conversion is lossless, lossy, conditional, or unknown. A canonical-looking example must not erase incompatible deployed behavior.

## 3. Count independence per capability

Maintain separate counts for deployed products, implementation families, and independent implementations of the behavior under review. Keep the observed project count visible even when several projects share one family.

Establish family relationships through repository ancestry, copied modules, shared protocol packages, and implementation history. Two products using the same serializer can show adoption of that serializer; they do not automatically show two independent implementations of its wire contract.

Assess this per capability. A fork can retain its parent's Offer implementation while independently replacing Payment. Record the evidence behind each relationship and mark unresolved ancestry as unknown. Do not count unresolved cases toward an independence threshold.

Report denominators explicitly: for example, “three observed deployments, two implementation families, one independent implementation verified for this capability.” Preserve minority variants even when they occur in only one family.

## 4. Demonstrate directional interoperability

For each capability, test whether producer A's output is correctly consumed by B at identified revisions. A→B and B→A are separate results. Only test directions applicable to the participants' roles; a consumer-only component need not become a producer to qualify.

Retain a matrix with states such as not assessed, predicted from code, fixture passed, live observed, failed, or blocked. Code prediction must never be displayed as an executed pass. A listing that renders successfully does not establish interoperable order processing or settlement.

Each executed comparison needs a fixture identifier, producer and consumer revisions, environment, procedure, expected result, actual result, and evidence. Include relevant update and failure behavior, not just an initial happy path. Record adapter requirements explicitly; translated compatibility differs from direct compatibility.

Use synthetic data and controlled environments by default. Illustrative JSON containing placeholders is not an executable test vector. A test involving signatures, identifiers, or encryption must satisfy the relevant requirements before its result can support a compatibility claim.

## 5. Rank facets within declared comparison cohorts

Publish separate views for capability coverage, implementation evidence, directional interoperability, production provenance, and build evidence. Avoid an overall “best marketplace” score. These views answer different questions and cannot compensate for one another.

Build evidence identifies the inspected revision, environment, dependency resolution, attempted procedure, outcome, and blockers. A local build pass does not demonstrate production deployment or protocol correctness. A blocked build is not automatically a product failure.

Before scoring, freeze a role profile and its applicable capability criteria. Store the profile version and digest, protocol baseline and digest, observation window, and source-scope policy in the cohort record. Comparisons require matching values. Scope policy includes whether the review covers a client alone or the complete client/service workflow.

Profiles can differ for classifieds, storefronts, auctions, or infrastructure. Exclude a criterion as not applicable only according to the frozen profile rules, with a recorded reason. Never remove a criterion after discovering that a project lacks it. Publish a new profile and reassess the cohort when the criteria need to change.

### Optional capability evidence score

Apply this rubric to small, explicit behavior criteria:

| Value | Required evidence |
| --- | --- |
| **0** | The capability is verified unavailable in the inspected scope, or a valid behavior test establishes a genuine failure. |
| **1** | A complete, reachable application trace establishes the behavior, with no unresolved contradictory evidence. |
| **2** | A complete reachable source trace plus a recorded, reproducible behavior test establishes a pass for the inspected implementation. |
| **null** | Evidence is documentation-only, unexamined, insufficient, conflicting, or blocked before the implementation can be established. |

An environmental block on optional execution does not erase a complete, uncontradicted source trace; that criterion may still receive 1 point.

An unsuccessful search does not establish zero. An invalid fixture, missing credential, or broken test environment does not establish a genuine behavior failure. Investigate contradictions; leave the criterion null until they are resolved. Split partially implemented capabilities into predetermined subcriteria instead of inventing fractional points.

For **N** applicable criteria, let **S** be the sum of known points and **U** the number of null criteria. Display the possible evidence-score interval as **S / (2N)** through **(S + 2U) / (2N)**, alongside the known/applicable count. This is a missing-evidence interval, not a statistical confidence interval or estimated product quality.

Do not assign an ordinal rank until every applicable criterion is known and the primary source commit is pinned. Synthetic examples are excluded. When **N = 0**, report the score as not applicable. The JSON eligibility flag covers mechanical source-evidence requirements; a coordinator must also review cohort comparability before placement. Rank complete reports only within the same frozen cohort; retain ties. Display production, build, and interoperability facets beside the score. Never award points for stars, NIP counts, kind counts, framework choices, or the number of integrations alone.

## 6. Turn observations into candidate decisions

Each proposed rule must solve a stated interoperability problem. Classify its starting point:

- **Observed pattern:** source-backed behavior awaiting further evaluation.
- **Interoperable candidate:** behavior supported by independent implementations and applicable cross-client tests.
- **Experimental extension:** useful behavior with limited implementation or adoption evidence.
- **Deliberate normative change:** a proposed improvement that differs from observed production behavior.

As a proposed gate for an interoperable core candidate, seek at least **two independent implementations of the behavior**, plus passing cross-client fixtures in the applicable producer/consumer directions. This is a research recommendation, not an assertion of an existing Foundation rule. Two branded deployments of one shared implementation do not satisfy it.

A deliberate normative change can be proposed without pretending that this adoption evidence exists. Identify it explicitly, supply a rationale and prototypes, define conformance tests, and explain migration costs. An unimplemented preference must remain labelled as such.

Passing these gates makes a proposal ready for review. It does not accept it. Popularity alone cannot settle incompatible variants, privacy problems, or an unsuitable trust model.

## 7. Prepare a decision packet

Use one packet for each coherent decision. Link evidence instead of copying large amounts of source code. Separate observations, inferences, and proposed requirements.

```markdown
# Decision <stable ID>: <short title>

Status: proposed | experimental | accepted | rejected | superseded
Owner and review venue:
Workflow stage and capability:
Report IDs, revisions, cohort, and evidence cutoff:

## Problem and affected participants
Describe the concrete interoperability problem and merchant/buyer impact.

## Observed variants
List wire variants, deployments, families, independent implementations,
contradictions, unknowns, and minority behavior with evidence links.

## Proposed contract
State exact fields, semantics, producer/consumer obligations, failure
handling, and relationship to the pinned NIP/specification baseline.

## Evidence and conformance tests
Link directional results, executable fixtures, prototypes, and gaps.
State whether this is observed convergence or a deliberate change.

## Alternatives and migration
Explain compatibility, adapters, version/capability negotiation, rollout,
fallbacks, old data, and reasons for retaining or retiring each variant.

## Privacy, trust, and operational effects
Document disclosure, retention, key handling, external dependencies,
merchant feedback, buyer feedback, and unresolved tradeoffs.

## Review and decision
Record reviewers, objections, responses, decision authority, dated outcome,
acceptance rationale, follow-up work, and superseded decision IDs.
```

## 8. Require an explicit community decision

Give implementers and affected merchants an opportunity to examine the proposed contract and fixtures. Seek feedback on practical order handling, failure recovery, payment reconciliation, privacy, and migration. Record unavailable feedback rather than inventing consensus.

Keep minority objections and alternatives visible. Explain why a common core, an optional extension, multiple supported profiles, or further experimentation best fits the evidence. “Most repositories do this” is useful context, not a complete acceptance rationale.

Acceptance requires an explicit, recorded decision through the Foundation's authorized review process. Do not invent a voting threshold or assume that an agent, a maintainer reply, a report merge, or silence constitutes acceptance. A report can be merged as research while its recommendations remain unaccepted.

Once accepted, link the resulting specification change, conformance tests, migration guidance, and decision record. Record deviations from the proposed research gates and the authorized rationale. An agent may prepare these materials; it must not silently turn its conclusions into normative text.

## 9. Preserve history and refresh deliberately

Published report IDs and decision IDs are append-only references. Corrections create a new revision that identifies what changed and what it supersedes. Preserve prior evidence and conclusions; never silently rewrite the historical basis of a ranking or accepted rule.

Recompute affected comparisons after a corrected report, changed profile, new protocol baseline, changed production provenance, or new interoperability result. State the new cutoff and preserve the previous comparison. Withdraw unsupported claims explicitly.

Recommend re-audits after material releases, protocol changes, deployment changes, or newly discovered conflicts. A recommendation does not create an automation. Each later audit must pin its own sources and distinguish changed behavior from improved evidence about unchanged behavior.
