# Open Markets Marketplace Research Kit

**Process version:** 1.0.0  
**Prepared:** 2026-11-05  
**Status:** proposed research process for community review

This kit tells an agent how to inspect a Nostr marketplace repository and produce evidence that the Open Markets community can compare, reproduce, and use to develop its specification.

The unit of research is **a specific implementation at a specific revision**. The main output is a supported account of its behaviour: the events it writes and reads, the commerce flows it implements, the services it depends on, and the limits of the available evidence.

Reports follow the four stages **Offer → Order → Payment → Resolution**. They preserve differences between implementations instead of making every client appear to follow the same draft.

## Start here

1. Read [AUDIT_GUIDE.md](AUDIT_GUIDE.md) before visiting the target repository.
2. Give the agent [AGENT_PROMPT.md](AGENT_PROMPT.md), with the target repository and output directory filled in.
3. Use [REPORT_TEMPLATE.md](REPORT_TEMPLATE.md) for the readable report and [report.schema.json](report.schema.json) for its structured companion.
4. Consult [STANDARDS_MAP.md](STANDARDS_MAP.md) for discovery leads, then pin and read the actual standards used in the audit.
5. Compare completed reports using [SYNTHESIS_GUIDE.md](SYNTHESIS_GUIDE.md).

[RESEARCH_NOTES.md](RESEARCH_NOTES.md) records the source checks behind the guide and an initial discovery queue for Shopstr, Plebeian Market, Conduit, Self-sown, Shosho, and Barattolo.

The repository owner can add this entire `research/` directory alongside the existing specification. It does not require replacing the specification's root README or installing an agent-specific skill.

## What each audit produces

Use an output directory such as:

```text
research/reports/<project>/<YYYY-MM-DD>-<commit-prefix>/
```

Save `report.md`, `report.json`, and any permitted, sanitized evidence or fixtures beneath it. Use `unresolved` in place of the commit prefix when the source revision cannot be established. Such reports remain useful discovery records but cannot enter a source-based ranking.

The report answers:

| Question | Required distinction |
| --- | --- |
| Is this the project's current source? | Canonical project, historical repository, fork, release-only repository, or unresolved identity |
| Is it used in production? | Project claim, observed deployment, and exact source-to-deployment binding |
| Which kinds and NIPs are used? | Kind, semantic purpose, wire variant, event layer, and individual read/write/validation operations |
| What does the marketplace actually do? | Reachable Offer, Order, Payment, and Resolution behaviour |
| What is it built with? | Declared dependency, resolved version, wired integration, and tested use |
| Can another client use its output? | A→B and B→A results for a specified capability and revisions |
| What can be standardized? | Repeated independent behaviour, incompatible variants, experiments, and missing evidence |

## Ranking policy

The kit supports **facet comparisons**: capability evidence, interoperability, deployment provenance, and build evidence. It does not define a universal product-quality score.

For a numerical capability comparison, select a profile **before** the audit. The included [listing-reader-v1 profile](profiles/listing-reader-v1.json) is a starting profile for clients that read classified listings. It is a research rubric, not an Open Markets conformance standard. Other roles need an explicitly agreed profile.

Each criterion receives `0`, `1`, `2`, or `null`: verified unavailable, source-traced, behaviour-tested, or unresolved. Unknowns stay visible and produce a score range. Ordinal placement is withheld until all criteria are resolved and the report satisfies the eligibility rules. A count of NIPs, stars, forks, downloads, or integrations is never a substitute for evidence.

## Validate a structured report

Use Python 3.10 or later with `jsonschema[format]>=4.18,<5` available in an isolated Python environment. Setup instructions are in [examples/README.md](examples/README.md). From the repository root:

```bash
python3 research/validate_report.py research/examples/report.example.json \
  --profile research/profiles/listing-reader-v1.json
```

For a report with `ranking: null`, omit `--profile`. For a ranked-profile report, supply the exact profile file whose SHA-256 is recorded in the report.

The validator checks the schema, identifiers and references, profile consistency, scoring arithmetic, and selected evidence requirements. It cannot verify whether a cited source really proves a claim. Human or independent agent review remains necessary.

The [example](examples/README.md) is synthetic. It is deliberately excluded from real marketplace rankings and adoption counts.

## Make the instructions easy for agents to discover

The repository owner can add this short pointer to an existing root agent-instructions file or research task description:

```text
For Nostr marketplace repository research, first read research/AUDIT_GUIDE.md.
Use research/AGENT_PROMPT.md and research/REPORT_TEMPLATE.md for each target.
Read research/SYNTHESIS_GUIDE.md before combining reports or proposing a spec change.
```

Keep any existing repository instructions and approval rules in place when adding the pointer.

## Research is an input to community decisions

An audit may propose a specification change. It cannot declare that the proposal has community approval, assign an official NIP, or merge a change merely because several repositories use a pattern. The synthesis process retains supporting evidence, dissent, migration costs, and conformance tests so reviewers can make a concrete decision.

The kit prepares local report files by default. Publishing reports, contacting maintainers, posting Nostr events, and making payments require a separately authorized task.
