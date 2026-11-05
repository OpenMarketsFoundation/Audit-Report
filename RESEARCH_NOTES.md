# Preparation notes and initial repository queue

**Checked:** 2026-11-05  
**Scope:** primary-source checks used to design this research process. These notes are not completed marketplace audits, production verification, or comparative rankings.

## 1. Pinned specification sources

- [Open Markets Foundation draft](https://github.com/OpenMarketsFoundation/specification/blob/24313d75bb6ebf3ba37266a504980340308b4dc8/README.md), commit `24313d75bb6ebf3ba37266a504980340308b4dc8`.
- [Official Nostr NIPs](https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/README.md), commit `98b57cf3e76f5de64a563350d437328e5d7a8f73`.

Both revisions were resolved through read-only Git access and inspected locally. [STANDARDS_MAP.md](STANDARDS_MAP.md) links the relevant pinned files. Each future audit must identify its own current and implementation-claimed standard revisions.

## 2. A source-level semantic conflict worth investigating

The following code was inspected at exact revisions. It demonstrates why an inventory must retain a kind's purpose, layer, and wire variant.

| Kind | Shopstr source | Plebeian source |
| --- | --- | --- |
| `30405` | `publishSavedForLaterEvent` constructs an event with encrypted cart/saved-data content. | `ProductCollectionSchema` defines a collection with product references. |
| `30406` | `BUYER_P2PK_ESCROW_EVENT_KIND` identifies buyer escrow records. | `SHIPPING_KIND` identifies shipping options. |

Source references:

- Shopstr [cart/saved event builder](https://github.com/shopstr-eng/shopstr/blob/2ed18bcd0db1f8ff4221bfd055f65e7f4a33f826/utils/nostr/nostr-helper-functions.ts#L399-L442).
- Shopstr [buyer escrow record module](https://github.com/shopstr-eng/shopstr/blob/2ed18bcd0db1f8ff4221bfd055f65e7f4a33f826/utils/cashu/p2pk-escrow-records.ts#L9-L12).
- Plebeian [product collection schema](https://github.com/PlebeianApp/market/blob/d4757b409b58e0ecf7c43e53629dd464c32dfb4e/src/lib/schemas/productCollection.ts#L5-L36).
- Plebeian [shipping option schema](https://github.com/PlebeianApp/market/blob/d4757b409b58e0ecf7c43e53629dd464c32dfb4e/src/lib/schemas/shippingOption.ts#L5-L19).

These observations establish the meanings present in those source locations. They do not establish complete application reachability, current deployment, or actual failure between the projects. A full audit should trace callers and consumers, inspect dispatch and filters, then test relevant fixtures. The possible outcomes include harmless isolation, deliberate dialects, dropped data, misinterpretation, or another result; the source snippets alone do not choose between them.

## 3. Initial discovery queue

This queue carries forward the marketplace projects relevant to Open Markets. Resolve identity, scope, source access, and production evidence afresh before each audit. No row is a verified production claim.

| Project | Starting repository | Research scope and qualification |
| --- | --- | --- |
| Shopstr | [shopstr-eng/shopstr](https://github.com/shopstr-eng/shopstr) | Application source. Preparation snapshot: `2ed18bcd0db1f8ff4221bfd055f65e7f4a33f826`. |
| Plebeian Market | [PlebeianApp/market](https://github.com/PlebeianApp/market) | Application source. Preparation snapshot: `d4757b409b58e0ecf7c43e53629dd464c32dfb4e`. Keep [the historical repository](https://github.com/PlebeianApp/plebeian-market-old) separate. |
| Conduit | [Conduit-BTC/conduit-mono](https://github.com/Conduit-BTC/conduit-mono) | Discover application boundaries and shared packages; pin its revision before support claims. |
| Self-sown | [shopstr-eng/self-sown](https://github.com/shopstr-eng/self-sown) | Source snapshot: `9ca51590daf6f6b8bc59f2453f398c293be8cb1a`. Resolve per-capability lineage before counting independence. |
| Shosho | [r0d8lsh0p/shosho-releases](https://github.com/r0d8lsh0p/shosho-releases) | A release-oriented starting point; establish source availability before a source audit. |
| Barattolo | [kreamistudio-crypto/barattolo-releases](https://github.com/kreamistudio-crypto/barattolo-releases) | At `9be9324b49cd2e27267bdfa0ba5b2303a2afedfa`, the [README](https://github.com/kreamistudio-crypto/barattolo-releases/blob/9be9324b49cd2e27267bdfa0ba5b2303a2afedfa/README.md) describes Android releases and source elsewhere. Discover the source or preserve the limitation. |

Additional discovery targets may include [LNbits Nostr Market](https://github.com/lnbits/nostrmarket) for legacy marketplace integrations and [Mostro](https://github.com/MostroP2P/mostro) for a separate P2P-exchange profile. Treat them as research leads, not members of an already comparable cohort.

## 4. What was and was not checked

Preparation used official specifications, project pages, and selected source files. No marketplace build was run, no purchase or payment was made, and no production interoperability exchange was performed. Source-level observations above are deliberately narrow. The synthetic JSON example included with the kit is a format demonstration and supplies no real marketplace evidence.
