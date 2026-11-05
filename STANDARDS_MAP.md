# Nostr standards discovery map

**Prepared:** 2026-11-05. **Purpose:** help auditors find protocol behavior and describe it precisely.

This is a discovery map, not an implementation checklist or a conformance certificate. The upstream [NIPs introduction][nips] also rejects treating NIPs as a checklist. Investigate the marketplace's actual use case, including project extensions and standards outside the NIPs repository. More supported kinds do not necessarily mean a better marketplace.

## Source baselines

| Source | Pinned revision | Role in an audit |
| --- | --- | --- |
| [nostr-protocol/nips][nips] | `98b57cf3e76f5de64a563350d437328e5d7a8f73` | Upstream definitions and status at this snapshot |
| [OpenMarketsFoundation/specification][omf] | `24313d75bb6ebf3ba37266a504980340308b4dc8` | A community marketplace draft to compare against observed implementations |

All NIP links below refer to the pinned revision. Before each audit, resolve and record the relevant current revisions and any older revision the target implementation claims to follow. Preserve both when they differ. Record the target repository commit independently. An upstream label describes that specification; it does not establish deployment, adoption, or compatibility.

## 1. Identify the event contract before assigning a NIP

[NIP-01][n01] defines the signed event envelope: integer `kind`, string `content`, string-array tags, author, timestamp, ID, and signature. Serialized JSON inside `content` remains a string on the wire. Addressable kinds `30000–39999` use the combination of kind, author, and `d` identifier; a particular event ID identifies one version. Ephemeral kinds occupy `20000–29999`; replaceable kinds include `10000–19999`, `0`, and `3`.

Keep event IDs, addressable coordinates, product IDs, and order IDs separate. Inspect replacement selection, same-time conflicts, stale caches, tag filtering, and incoming-event validation. Do not infer a feature from an enum, an SDK dependency, or a matching number. NIP numbers and event kinds are different namespaces.

For every observed protocol, record the producer, consumer, transport, envelope layers, exact payload, validation, and resulting application state. Preserve custom fields and unfamiliar kinds rather than forcing them into the nearest familiar standard.

## 2. Commerce and messaging

“Draft” and “optional” below reproduce upstream status labels; they are not audit conclusions.

| Source | Discovery targets | Pinned status | What to distinguish |
| --- | --- | --- | --- |
| [NIP-99][n99] | `30402` listings; `30403` drafts/inactive listings | Draft; optional | Markdown description, metadata tags, publication versus consumption, active/sold state. A draft kind alone does not establish confidentiality. |
| [NIP-15][n15] | `30017` stalls; `30018` products; `30019` marketplace UI; `30020` auctions; `1021` bids; `1022` confirmations | Draft; optional; **unrecommended**, referring to NIP-99 | Legacy interoperability remains evidence. Inspect product/stall references, checkout payloads, bid author checks, and version-specific auction references. |
| [NIP-09][n09] | `5` deletion requests | Draft; optional; relay | Requests can reference IDs or addresses. Verify author checks and consumer/cache handling; publication of a request does not prove global deletion. |
| [NIP-04][n04] | `4` encrypted messages | Final; optional; relay; **unrecommended**, referring to NIP-17 | Identify encryption and recipient tags. NIP-15 checkout message `type` values are payload fields, not additional event kinds. |
| [NIP-17][n17] | Inner `14` messages, `15` file messages, `7` reactions; `10050` inbox relays | Draft; optional; relay | Record inbox routing, sender/recipient copies, message parsing, and seal/rumor author matching. Custom commerce payloads need their own definitions. |
| [NIP-44][n44] | Versioned encryption; current defined version `0x02` | Optional | Defines encryption, not a new message kind. Finding encryption calls does not prove a complete messaging protocol. |
| [NIP-59][n59] | Unsigned rumor, `13` seal, `1059` gift wrap | Optional; relay | Wrapping is reusable across applications. Finding `1059` does not establish which business protocol is inside. |
| [NIP-37][n37] | `31234` encrypted draft wraps; `1234` checkpoints; `10013` private-content relays | Draft; optional | Distinguish encrypted draft storage from inactive public listings and browser-only drafts. |

### Preserve the layers

For gift-wrapped commerce, inspect the **business payload inside an unsigned rumor**, its encrypted and signed **seal**, and the encrypted **outer gift wrap** carried to relays. Record each layer separately, with links to construction and consumption code. A receiver needs to validate and interpret the inner message as well as obtain the outer event. These distinctions follow [NIP-59][n59] and [NIP-17][n17].

Suggested audit attributes are `outer_kind`, `inner_kind`, `envelope`, `encryption_scheme`, `encryption_version`, `transport`, `producer_role`, and `consumer_role`. Keep separate evidence for constructing, sending, fetching, decrypting, parsing, validating, and applying an event. Do not count every layer as a separate completed marketplace feature.

## 3. Wallets, payments, signing, and authentication

| Source | Discovery targets | Pinned status | What to distinguish |
| --- | --- | --- | --- |
| [NIP-47][n47] | `13194` wallet information; `23194` requests; `23195` responses | Draft; optional | Core NWC wallet RPC. Inspect the actual methods, authorization, response handling, and checkout call path. Extensions have separate specifications. |
| [NIP-57][n57] | `9734` zap requests; `9735` receipts | Draft; optional | The request goes to the LNURL callback, rather than normal relay publication. Inspect receipt validation and any explicit connection to an order. |
| [NIP-60][n60] | `17375` wallet; `7375` unspent proofs; `7376` history | Draft; optional | Cashu wallet state is distinct from transferring funds or settling a purchase. |
| [NIP-61][n61] | `10019` receiving information; `9321` nutzaps | Draft; optional | Inspect accepted mints, recipient locking, receipt processing, and redemption. A Cashu import alone proves none of these. |
| [NIP-07][n07] | Browser `window.nostr` methods | Draft; optional | Public-key access and signing are core; encryption methods are optional. Record individual methods and reachable integration paths. |
| [NIP-46][n46] | `24133` remote-signing requests/responses | No status badges in this file | Separate client, remote-signer, and user identities. Record supported methods and the encryption revision actually used. |
| [NIP-55][n55] | Android signer communication | Draft; optional | Record supported platform and transport; do not infer Android integration from browser signing. |
| [NIP-42][n42] | `22242` in relay `AUTH` messages | Draft; optional; relay | This is authentication, not a normal published/queryable event. Inspect challenge, relay binding, and retry handling. |
| [NIP-98][n98] | `27235` HTTP authorization | Draft; optional | Inspect URL, method, time, and applicable body binding. Keep HTTP authentication separate from relay AUTH. |

For payment findings, trace the transition from invoice or token creation through verification to the order state change. Report which actor verifies what, how amount and recipient match the order, and what happens on duplicate, failed, or delayed responses. “Invoice created,” “payment verified,” “order accepted,” and “fulfilled” are different observations. Signing support likewise does not establish custody arrangements; investigate who controls the relevant keys and funds.

## 4. Relays, discovery, media, and other marketplace profiles

| Source | Discovery targets | Pinned status | What to distinguish |
| --- | --- | --- | --- |
| [NIP-11][n11] | Relay information over HTTP | Draft; optional; relay | A relay's `supported_nips` is its declaration, not evidence of the marketplace client's behavior. |
| [NIP-65][n65] | `10002` read/write relay lists | Draft; optional | Rendering a list differs from using it to route publication and retrieval. |
| [NIP-89][n89] | `31989` app recommendations; `31990` handler information | Draft; optional | Discovery and routing declarations do not prove that the recommended application implements every advertised kind. |
| [NIP-85][n85] | `30382–30385` trusted assertions about different subjects | Draft; optional | Do not equate a project's generic “reviews” label with this assertion protocol; inspect its actual kind, subject, provider, and fields. |
| [NIP-50][n50] | `search` in subscription filters | Draft; optional; relay | Relay search differs from local filtering or a centralized search API. Record fallbacks. |
| [NIP-92][n92] / [NIP-94][n94] | `imeta` tags / `1063` file metadata | NIP-92: no status badges; NIP-94: draft, optional | Metadata and file transport are separate. Image URLs alone do not prove either implementation. |
| [NIP-96][n96] | HTTP file-storage API | Draft; optional; **unrecommended**, referring to NIP-B7 | Record legacy upload support if implemented; do not rename it Blossom. |
| [NIP-B7][nb7] | Blossom; `10063` server lists | Draft; optional | Inspect upload, discovery, fallback retrieval, and hash verification separately. Pin any relevant external BUDs too. |
| [NIP-69][n69] | `38383` P2P exchange orders | Draft; optional | Analyze exchange liquidity/order behavior within that marketplace profile. |
| [NIP-90][n90] | `5000–5999` computation requests; `6000–6999` results; `7000` feedback | Draft; optional; **unrecommended** | Computation marketplace; result kind is request kind +1000. Inspect semantics instead of assigning support from range membership. |

## 5. Open Markets draft: separate comparison track

The pinned [OMF draft][omf] proposes `30405` collections, `30406` shipping, `31555` reviews, and inner `16` order messages / `17` payment receipts. These are project proposals, not canonical NIP-17 message definitions. It lists collections as both required and optional: record an unresolved requirement question rather than silently choosing. Its price-frequency examples use duration letters such as `D`, while [NIP-99][n99] uses nouns such as `day`; preserve the difference and test interpretation. NIP-99's e-commerce extension link targets **GammaMarkets/market-spec**. Resolve that project's revision separately; it is not the OMF revision.

Additionally, upstream [NIP-18][n18] uses kind `16` for generic reposts, and [NIP-25][n25] uses `17` for external-content reactions. Flag contextual kind reuse, dispatch assumptions, and collisions for community review. An encryption wrapper does not, by itself, demonstrate compatible inner semantics.

## 6. Carry observations into the report

Use a standard name as a hypothesis until source and behavior substantiate it. Preserve claimed support, observed implementation, tested exchange, and unresolved compatibility separately. Record whether a behavior is default, conditional, disabled, or unreachable. An inaccessible backend or encrypted payload leaves an evidence gap; it is not proof of absence.

When synthesizing reports, compare equivalent business operations and exact contracts. Preserve incompatible variants, older revisions, and custom extensions. Recommend a community requirement only with traceable implementation evidence, explicit tradeoffs, and a reviewable interoperability test. This map establishes neither marketplace rankings nor community consensus.

[nips]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/README.md
[omf]: https://github.com/OpenMarketsFoundation/specification/blob/24313d75bb6ebf3ba37266a504980340308b4dc8/README.md
[n01]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/01.md
[n04]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/04.md
[n07]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/07.md
[n09]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/09.md
[n11]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/11.md
[n15]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/15.md
[n17]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/17.md
[n18]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/18.md
[n25]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/25.md
[n37]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/37.md
[n42]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/42.md
[n44]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/44.md
[n46]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/46.md
[n47]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/47.md
[n50]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/50.md
[n55]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/55.md
[n57]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/57.md
[n59]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/59.md
[n60]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/60.md
[n61]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/61.md
[n65]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/65.md
[n69]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/69.md
[n85]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/85.md
[n89]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/89.md
[n90]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/90.md
[n92]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/92.md
[n94]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/94.md
[n96]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/96.md
[n98]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/98.md
[n99]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/99.md
[nb7]: https://github.com/nostr-protocol/nips/blob/98b57cf3e76f5de64a563350d437328e5d7a8f73/B7.md
