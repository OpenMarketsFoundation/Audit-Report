# Marketplace audit: [Project name]

> Copy this file into a report directory and replace the instructions and placeholders. Keep every section. Use `unknown`, `not assessed`, or `not applicable — [reason]` when appropriate. Use the exact JSON enums in structured data. Do not leave example evidence IDs as if they were real findings.

## 1. Audit identity and scope

| Field | Value |
| --- | --- |
| Report ID / revision | [Stable ID] |
| Audit timestamp | [RFC3339, including timezone] |
| Process / prompt / schema version | 1.0.0 / 1.0.0 / 1.0.0 |
| Research agent and reviewer | [Identity/version when available; reviewer or not yet reviewed] |
| Canonical project / repository | [Name and URL] |
| Source commit | [Full SHA or unknown] |
| Requested and resolved branch/tag | [Both] |
| Project roles | [Listing reader, merchant publisher, commerce buyer/seller, auction, exchange, infrastructure, other] |
| Included applications/components | [Paths and purposes] |
| Additional repositories/dependencies | [URLs, resolved revisions, and role] |
| License evidence | [Observed license and source; unknown if unresolved] |
| Allowed execution / observation | [Exact scope] |
| Comparison profile / SHA-256 | [Exact profile and digest, or none] |
| Comparison cohort / cutoff / window | [Frozen cohort metadata, or none] |
| Protocol baseline / digest | [Pinned standards and cohort baseline, or not defined] |
| Previous or superseded report | [ID/path or none] |

Describe source availability, excluded components, access limits, and the effect of exclusions on conclusions. Identify whether this is a complete source audit, partial source audit, or documentary/black-box report. Distinguish current code from historical or release-only repositories.

## 2. Findings at a glance

Write a short, evidence-backed overview: what this revision does, its distinctive wire variants, the strongest compatibility finding, the largest uncertainty, and its relevance to a community specification.

| Finding | Claim IDs | Evidence IDs | Boundary or limitation |
| --- | --- | --- | --- |
| [Observed behaviour] | [C…] | [E…] | [What remains unproven] |

State production provenance, build result, and interoperability status separately. Do not call the product production-verified solely because a website exists. Do not present proposed changes as accepted requirements.

## 3. Production, releases, and source binding

| Deployment / release | Environment | Provenance | Observation time | Observed behaviour | Deployed commit / binding | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [URL or artifact] | [production/staging/demo/unknown] | [unverified/project_claimed/observed/commit_linked] | [Timestamp or unknown] | [Specific action/page] | [SHA or unknown; binding explanation] | [E…] |

Record failed or blocked observations without inferring global unavailability. List release evidence separately where it does not establish a live deployment. Record usage or transaction metrics only with their own provenance and definitions; otherwise use unknown.

## 4. Architecture, build, and runtime

Explain application boundaries and the path from interface to Nostr, services, wallets, persistence, and media. Identify which state is on Nostr, in a backend, in local storage, or handled manually.

| Component | Language/runtime/framework | Resolved version source | Role | Evidence |
| --- | --- | --- | --- | --- |
| [Component] | [Stack] | [Lockfile or other evidence] | [Purpose] | [E…] |

| Build/test check | Commit and component | Environment and configuration | Exact command | Status | Outcome / blocker | Log or evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [Check] | [SHA/path] | [OS, architecture, runtime, package manager, network] | [Command or not attempted] | [not_attempted/blocked/failed/passed] | [Bounded result] | [E…] |

Distinguish installation, compilation, test execution, startup, and any actual artifact-hash comparison. List prerequisites and missing services without disclosing secrets.

## 5. Integration inventory

| Category / integration | Declared version | Resolved version | Application path and purpose | Status | Default/optional and service boundary | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [Signer/wallet/relay/SDK/media/backend/indexer/storage/etc.] | [Range or unknown] | [Version or unknown] | [Entry point → use] | [dependency_present/wired/locally_verified/deployment_verified/unknown] | [Conditions and dependency] | [E…] |

Record key/signing authority, payment custody or mint/service dependence when established, and recovery or portability limitations. Avoid conclusions about safety or decentralization that the evidence does not support.

## 6. Declared standards versus observed behaviour

| Standard / project extension | Exact baseline and status | Project's support claim | Observed scope | Deviations / unresolved questions | Claims and evidence |
| --- | --- | --- | --- | --- | --- |
| [NIP or project specification] | [URL, SHA, file, status] | [Paraphrased claim or none] | [Specific operations and fields] | [Differences] | [C…; E…] |

Keep official NIPs, external standards, OMF draft proposals, and project-specific dialects separate. Do not upgrade an imported SDK, a kind constant, or a README label into application conformance.

## 7. Event-kind and wire-variant inventory

### 7.1 Inventory

Use one row per kind, semantic purpose, variant, and layer. Add a separate operation row whenever evidence differs by direction or behaviour.

| Event ID | Kind / symbol | Purpose / capability | Layer and transport | Variant / standard | Availability | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [EV…] | [Integer / symbol] | [Actual use] | [Outer, seal, rumor, AUTH, HTTP, local] | [Variant ID / standard ID] | [default/opt_in/experimental/disabled/unknown] | [E…] |

| Event ID | Operation | Implementation status | Execution status | Reachable path | Claims / evidence |
| --- | --- | --- | --- | --- | --- |
| [EV…] | [publish/consume/parse/validate/render/store/sign/encrypt/decrypt/authenticate/etc.] | [Exact implementation enum] | [Exact execution enum] | [Entry → handler → effect] | [C…; E…] |

### 7.2 Contract record — repeat per material variant

**Variant:** [ID and meaning]  
**Producers and consumers:** [Components and roles]  
**Application/source baseline:** [Revisions]  
**Standard relationship:** [Conforms within stated scope, extension, divergence, or unknown]

| Contract item | Writer emits | Reader accepts/requires | Defaults / units / lifecycle | Evidence and unknowns |
| --- | --- | --- | --- | --- |
| Content | [Encoding and structure] | [Parser rules] | [Version] | [E…] |
| Tags/fields | [Names, types, positions, multiplicity] | [Required/optional/tolerance] | [Missing/empty/duplicate behaviour] | [E…] |
| Identity/references | [IDs, coordinates, authors, targets] | [Resolution/author checks] | [Version/reference semantics] | [E…] |
| Price/payment | [Amount and asset format] | [Validation and matching] | [Precision, conversion, expiry, fees] | [E…] |
| Lifecycle | [Create/update/withdraw messages] | [Replacement/cache/state behaviour] | [Stale/duplicate/replay handling] | [E…] |
| Private transport | [Envelope and recipients] | [Decryption/identity verification] | [Encryption version and routing] | [E…] |

Link an illustrative shape or executable fixture where useful. Label its category and provenance. Preserve conflicting fields and candidate kind collisions explicitly.

## 8. Commerce workflow

| Stage | Capability / action | Actor and entry point | Event variant or API | State transition and result | Status | Claims / evidence |
| --- | --- | --- | --- | --- | --- | --- |
| Offer | [Create/read/update/withdraw/etc.] | [Actor/path] | [Variant/service] | [Observed rule] | [Status] | [C…; E…] |
| Order | [Request/accept/reject/status/etc.] | [Actor/path] | [Variant/service] | [Observed rule] | [Status] | [C…; E…] |
| Payment | [Request/proof/verification/etc.] | [Actor/path] | [Variant/service] | [Observed rule] | [Status] | [C…; E…] |
| Resolution | [Fulfilment/cancel/refund/dispute/review/etc.] | [Actor/path] | [Variant/service] | [Observed rule] | [Status] | [C…; E…] |

Distinguish the result of each step. A readable listing is not an accepted order; an invoice is not settlement; a receipt is not fulfilment. Explain manual steps and unavailable external-service logic.

## 9. Directional interoperability

| Capability / variant | Producer and revision | Consumer and revision | Direction | Status | Expected versus actual behaviour | Fixture / evidence / limits |
| --- | --- | --- | --- | --- | --- | --- |
| [Capability] | [A + SHA] | [B + SHA] | [A→B] | [not_assessed/code_predicted/fixture_passed/live_observed/failed/blocked] | [Result] | [IDs and conditions] |

Record direct compatibility separately from compatibility through an adapter. State whether an independent consumer was tested and whether the fixture came from the actual producer. Include applicable negative/update cases. If no cross-client test was run, say so explicitly.

## 10. Test and search record

| Check ID | Purpose | Procedure, source paths, or search | Fixture/environment | Expected result | Actual result/status | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [T…] | [Question] | [Exact reproducible procedure] | [IDs/configuration] | [Expected] | [Observed] | [E…] |

Record exclusions, unavailable code, dynamic/configuration paths, blocked prerequisites, and tests found but not executed. A `not_found_in_scope` statement must point to its bounded search record.

## 11. Claim ledger

| Claim ID | Capability ID | Bounded statement | Statement type | Implementation | Execution | Availability | Evidence | Limits / counterevidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| [C…] | [Stable capability] | [Precise claim] | [observed/inferred/proposed] | [Exact enum] | [Exact enum] | [Exact enum] | [E…] | [Limits] |

Use `implemented`, `partial`, `declared_only`, `not_found_in_scope`, `not_implemented`, `unknown`, or `not_applicable` for implementation. Use `not_run`, `passed`, `failed`, `blocked`, or `not_applicable` for execution. Do not claim a test pass merely because a test definition exists.

## 12. Comparison and ranking facets

**Profile:** [ID, exact SHA-256, role, and fixed criteria — or none]  
**Cohort:** [ID, baseline, cutoff/window, and source-scope policy — or none]

| Criterion ID | Capability | Points | Supporting claims | Reason / unknowns |
| --- | --- | --- | --- | --- |
| [Profile criterion] | [Capability] | [0/1/2/null] | [C…] | [Evidence anchor] |

**Evidence range:** [lower]–[upper] out of [maximum].  
**Known criteria:** [known]/[total]; evidence completeness [percentage].  
**Mechanical eligibility:** [true/false and reason].  
**Cohort review / ordinal placement:** [Not yet compared; tie/rank only in a reviewed corpus].

| Separate facet | Result | Evidence and limits |
| --- | --- | --- |
| Deployment provenance | [State] | [Evidence] |
| Build/reproduction | [Component-specific result] | [Evidence] |
| Directional interoperability | [Capability-specific result] | [Evidence] |
| Independent implementation relevance | [Family/independence assessment] | [Evidence] |

If no profile was chosen, use `ranking: null` in JSON and explain that only descriptive facets are reported. Unknowns do not become zero; do not change the denominator or create an overall quality rating.

## 13. Lineage and specification candidates

### Implementation lineage

| Capability IDs | Implementation family | Independent/shared/unknown | Relationship and evidence |
| --- | --- | --- | --- |
| [Capabilities] | [Family ID] | [Assessment] | [Origin, shared parser/serializer, divergence] |

### Candidate — repeat per coherent issue

**Candidate ID / status:** [Stable ID; observed pattern, conflict, experimental, candidate, or needs evidence]  
**Problem:** [Concrete interoperability or merchant/buyer problem]  
**Observed variants:** [Facts and claim/evidence IDs]  
**Proposed behaviour:** [Recommendation, or no recommendation yet]  
**Independent support and counterexamples:** [Per-capability families, not raw repository count]  
**Standard relationship:** [Pinned upstream/draft requirements and conflicts]  
**Migration and trust implications:** [Old data, adapters, privacy, external dependencies]  
**Suggested conformance test:** [Input, expected result, applicable directions]  
**Next evidence:** [Concrete missing source, test, deployment binding, or feedback]

These are research inputs. They do not establish community acceptance.

## 14. Conflicts, unknowns, and next actions

| Question | Conflicting or missing evidence | Effect on conclusions | Next evidence that would resolve it |
| --- | --- | --- | --- |
| [Question] | [Evidence/limitation] | [Blocked claim, score, compatibility, etc.] | [Specific next step] |

Prioritize unresolved issues by their effect on interoperability and specification design. Do not contact anyone or run production actions unless separately authorized.

## 15. Evidence ledger

| Evidence ID | Type | Immutable location or retained artifact | Revision / time | Symbol/locator and what was inspected | What it supports / limits |
| --- | --- | --- | --- | --- | --- |
| [E…] | [source/documentation/test_run/etc.] | [Exact link/path] | [SHA or observation time] | [Path, lines, symbol, procedure] | [Bounded support] |

For retained fixtures/logs, include provenance, transformation/redaction notes, and checksums where appropriate. Do not publish secrets or private order data. A mutable URL requires an observation time and an explicit reproducibility limitation.

## 16. Delivery and review

- Structured validation: [Command, outcome, and validator version].
- Markdown/JSON agreement: [Checked or outstanding differences].
- Source and evidence review: [Reviewer/status].
- Sensitive-data and fixture-label check: [Result].
- Delivered files: [Relative paths].
- Remaining blockers: [List or none].

### Structured-data mapping

Use the schema's `target`, `scope`, `standards`, `deployments`, `builds`, `integrations`, `event_kinds`, `interoperability`, `claims`, `lineage`, `candidates`, `evidence`, and `ranking` fields. Store additional narrative details as precise claims/evidence, contract details, or scoped limitations rather than adding undocumented top-level JSON fields. Include the process/prompt versions and cohort metadata. The schema is authoritative for field names and enums; this Markdown template supplies the human-readable review structure.
