# Machine-readable report example and validation

**`report.example.json` is entirely synthetic.** It is a format demonstration, not a marketplace audit. Every repository URL, commit identifier, source locator, dependency, test result, and candidate finding is invented. `example.invalid` locations are placeholders, not evidence to retrieve.

Keep `record_type: "example"`. Exclude this file from adoption counts, interoperability matrices, comparison cohorts, and rankings. A real audit needs newly collected evidence; changing the record type does not turn this example into one.

## Validate the example

Use Python 3.10 or later. Install the validator dependency in your normal isolated Python environment:

```bash
python3 -m pip install "jsonschema[format]>=4.18,<5"
```

From the package root:

```bash
python3 research/validate_report.py research/examples/report.example.json \
  --profile research/profiles/listing-reader-v1.json
```

Expected result: a structural/semantic validation pass, an explicit synthetic-example note, a score interval of **4–8 out of 12**, coverage of **4/6 (66.67%)**, and `eligible: false`.

The validator reads the specified JSON, the supplied profile, and the adjacent schema. It does not install or execute target repository code, access credentials, contact relays, or fetch evidence URLs. A `PASS` means the record is internally consistent. It does not prove the cited source exists, that a path is reachable, that a reported test ran, that two clients interoperate, or that a deployment uses the audited commit.

Exit codes are `0` for a consistent record, `1` for validation failure, and `2` for a dependency, input, or JSON-reading problem. Duplicate JSON object keys and non-standard `NaN`/infinity values are rejected.

## File responsibilities

| File | Purpose |
| --- | --- |
| `../report.schema.json` | JSON Schema Draft 2020-12, version `1.0.0`, defining the report contract. |
| `../validate_report.py` | Checks schema, duplicate IDs, cross-references, evidence anchors, profile identity, and score arithmetic. |
| `../profiles/listing-reader-v1.json` | A fixed, behavior-only profile with six complete listing-reader criteria. |
| `report.example.json` | A simulated populated record showing `0`, `1`, `2`, and `null` without asserting actual support. |

## Report conventions

`schema_version`, `process_version`, and `prompt_version` are required and set to `1.0.0`. `report_id` identifies one immutable report snapshot; `previous_report_id` may link an earlier report. `audited_at` and observation times use RFC 3339 with a timezone. A commit is a full lowercase Git SHA-1 or Git SHA-256 identifier; use `null` when unresolved.

All inventory arrays are present, even when empty. An empty array means there are no recorded entries; it does not itself prove that a project lacks that capability. Explain inspection gaps in `scope.limitations`. A narrative report can use `ranking: null` if no suitable profile was declared before the audit.

The report separates implementation, execution, and availability:

| Field | Example meaning |
| --- | --- |
| `implementation: "implemented"` | A complete reachable implementation is evidenced for the stated claim. |
| `execution: "blocked"` | Behavior could not be run under the recorded environment constraints. This does not undo existing source evidence. |
| `availability: "opt_in"` | The implementation requires a recorded configuration or user choice. |
| `implementation: "not_found_in_scope"` | A bounded inspection found no evidence. This does not prove non-implementation. |
| `statement_type: "proposed"` | A suggested future rule or change, which cannot support a non-null score. |

An `event_kinds` row represents **kind + purpose + layer + variant**, with separate operations. Equal kind numbers are not assumed to share a schema. Allowed layers include relay events, seals, rumors, application payloads, authentication events, HTTP authorization, and local objects. Put the actual transport in `contract`, such as normal relay `EVENT`, relay `AUTH`, an HTTP authorization header, an encrypted inner payload, or a local-only object. `contract` may be a concise string or a structured object containing tags, content, identity, validation, transitions, encryption, and dependencies.

Interoperability rows have `producer` and `consumer` objects, each with `name`, `repository`, `commit`, and `role`. Record `capability_id`, `variant`, and an explicit `direction`. A repository or revision can be `null` when unknown. A passing test for one reader does not establish producer-to-consumer compatibility.

## Evidence anchors

Each evidence record has an ID, type, location, revision, locator, observation time, summary, and limitations. Use immutable source links and exact symbols/lines in real reports. Evidence locations can also identify bundled logs or fixtures. A `test_definition` records that a test exists; a `test_run` records an actual execution and its bounded result. Record command, environment, expected/actual result, and outcome in the linked run artifact and summary.

Evidence requirements also apply when `ranking` is null: implemented claims need source evidence, partial/unavailable claims need source or behavior evidence, declared-only claims need an attributable declaration, and search misses need a bounded search record. Implemented event operations must link complete implemented claims. Wired, locally verified, and deployment-verified integration labels require the corresponding source, execution, or deployment-observation evidence type. These checks establish record consistency, not the truth of an agent's labels.

For scored claims, at least one qualifying application source record must match `target.commit` when that commit is known. A qualifying test run must likewise identify that audited revision. Dependency evidence may retain its own pinned revision; the validator does not require every dependency to have the application's commit ID.

Both `observed` and `commit_linked` deployment provenance require a recorded observation time and `deployment_observation` evidence. `commit_linked` additionally requires a non-null `deployed_commit` and `deployment_binding` evidence whose `revision` matches it. An artifact binding alone does not prove a live deployment. If the deployed commit differs from the primary audited commit, validation reports that mismatch. The binding still needs human verification; the validator cannot establish that an attestation or build manifest is authentic.

## Fixed-profile scoring

Declare the exact profile before inspection and retain its exact file bytes. The `listing-reader-v1` criterion IDs are `discover`, `render`, `price`, `address`, `update`, and `withdrawal`, in that order. Each maps to the same name prefixed with `listing.` as its capability ID.

The withdrawal criterion assesses the complete declared variant, which may honor a sold/inactive state, a valid deletion request, or another explicit convention. This profile does not impose one commerce protocol or imply erasure from remote systems.

| Points | Required evidence |
| --- | --- |
| `0` | Explicitly verified non-implementation, or a genuinely failed behavior run. A bounded search miss or environment failure is insufficient. A pinned failed behavior run may support `0` even without source evidence. |
| `1` | A complete reachable implementation trace, with no contradictory evidence. Behavior execution is `not_run` or `blocked`. |
| `2` | A complete reachable implementation trace and a recorded passing behavior run. Both source and test anchors are required. |
| `null` | Unknown, unexamined, documentation-only, partial, conflicting, or blocked without a complete source trace. |

Do not award fractional points to a partial implementation. If a criterion is too broad, design a narrower profile version before a later comparison and re-audit under that profile. Keep every criterion in the denominator; neither `not_applicable` nor missing data removes a row.

For `N` fixed criteria and `U` unknown entries:

- `lower = sum(non-null points)`
- `upper = lower + 2 × U`
- `max = 2 × N`
- `known = N − U`
- `coverage_percent = round(100 × known / N, 2)`

Unknown is not a zero. The lower and upper values express the interval left by incomplete evidence. Do not sort incomplete reports by the lower value as if it were a completed score.

The validator checks that the score rows match the profile IDs **and order**, and that each linked claim has the criterion's capability ID. It checks the evidence types and recorded revisions that can justify each score; it cannot check the completeness or truth of a source trace, distinguish a falsely labelled environment failure from a behavior failure, or detect an omitted contradiction. Those are reviewer responsibilities.

### Profile digest

`ranking.profile_sha256` is the SHA-256 of the profile's **exact bytes**, including whitespace and the final newline. For example:

```bash
python3 - <<'PY'
from pathlib import Path
from hashlib import sha256
path = Path('research/profiles/listing-reader-v1.json')
print(sha256(path.read_bytes()).hexdigest())
PY
```

Do not silently update a profile in the middle of a cohort. A changed digest establishes a different comparison baseline, even if the filename and profile ID were accidentally retained.

## Eligibility and cohorts

`ranking.summary.eligible` is **mechanical source-evidence eligibility**, not a global rank. It can be true only for `record_type: "audit"`, a pinned primary commit, and 100% known criteria. For a `spec_conformance` profile, every profile-required standard must also be present and commit-pinned. A behavior-only profile does not imply specification conformance.

Ordinal placement additionally requires a reviewed, comparable cohort. The optional `cohort` object preserves its `id`, `comparison_cutoff`, `observation_window`, `source_scope_policy`, `baseline_ids`, and `baseline_digest`. `baseline_ids` refer to entries in `standards`. `baseline_digest` is the SHA-256 of the exact bytes of the coordinator's retained baseline manifest, containing the relevant URLs, commits, paths, statuses, and any interpretation decisions; no canonicalization is assumed. Use `null` when no retained manifest is available. This digest is distinct from the exact-byte profile digest and is preserved, not recomputed, by this validator. A missing or unresolved cohort remains unsuitable for ordinal placement even when mechanical eligibility is true. There is deliberately no numeric rank field in a single report.

The machine-readable checks do not decide implementation-family independence, adoption denominators, publication approval, or community acceptance of a proposed rule. Keep those decisions in the synthesis and review process.
