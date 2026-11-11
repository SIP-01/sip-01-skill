---
name: sip-01
description: Build, validate and consume SIP-01 kind 39697 Web Index Observation events on Nostr. Use for URL normalization, d/x hashing, event construction and validation, crawler/relay/search-engine integration, and SIP-01-specific query semantics. Do not use as a generic Nostr signing/key-management guide or as a source for current NIP text; use the nostr and nostr-nips skills for those tasks.
---

# SIP-01 Search Index Protocol skill

## Status and source of truth

SIP-01 is a draft, optional Nostr protocol profile. It is not an accepted NIP. The canonical specification is https://github.com/SIP-01/SIP-01/blob/main/public/spec/SIP-01.md.

The reference snapshot in this package is refreshed to canonical specification revision v1.3 (2026-11-07). The v1.3 update adds optional extension conventions for role/proto and a companion capability-announcement reference; it does not change kind 39697, schema v=1 or existing observations. Always check the canonical repository before relying on details from a snapshot.

The kind 39698 capability profile described in the companion document is a SIP-01 ecosystem extension, not a universally accepted Nostr standard and not a replacement for kind 39697.

## Activation and scope

Use when a task involves:
- kind 39697 Web Index Observation events;
- SIP-01 URL normalization, d document identity or x content hash;
- building or validating crawler observations;
- consuming, grouping or ranking decentralized web-index results;
- implementing a SIP-01-aware relay or search client;
- kind 16919 crawler heartbeat semantics used by the indexstr implementation.

Do not use this skill for generic Nostr key custody, signing or NIP-44 cryptography. Do not treat NIP-50 extensions, NIP-11 relay metadata or kind 39698 capability discovery as mandatory support on stock relays.

## Event model

One addressable event per (indexer pubkey, normalized URL). The event says that an indexer observed a public web document at a time and supplies lightweight metadata.

Core fields:
- kind: 39697
- d: widx: plus the first 32 hexadecimal characters of SHA-256 of the normalized URL
- u: normalized canonical URL
- x: SHA-256 of the published title, newline, and published description
- v: schema version 1
- alt: human-readable event description
- content: JSON with the title and optional description

The exact cardinality, length limits, normalization and hash rules are defined by the canonical spec. Do not reconstruct them from memory.

## Task routing

| Task | Procedure |
|---|---|
| Normalize URL, compute d/x, build or validate an event | Run scripts/sip01.py; run its selftest before changing protocol code. |
| Sign/publish an event | Send the unsigned event to a dedicated indexer signer using NIP-07/NIP-46 or a reviewed signer library. The skill agent must not receive a raw nsec or pass it as a command argument. |
| Query or inspect relays | Use a Nostr client or the Nostr skill's relay query helper. Treat timeout, CLOSED and EOSE without NIP-67 finish as partial/unknown, not exhaustive. |
| Exact current fields and normative rules | Read references/spec.md and then verify the current canonical specification URL above. |
| Integration steps | Read references/implementation-guide.md. |
| Ecosystem repos, NIP-50 and NIP-11 profile | Read references/ecosystem.md. |

## Non-negotiable rules

1. Byte compatibility matters. Use scripts/sip01.py or the canonical reference implementation. Run selftest (spec §13 vectors) after protocol changes.
2. Recompute d from normalized u and verify it. When x is present, recompute it from the exact title and description that were published, including truncation.
3. Indexer identity is separate from a human user's account identity. Use one isolated identity per node/indexer. Do not reuse the user's social identity.
4. Never place private keys in prompts, agent context, shell history, command arguments, logs, source control or ordinary stdout. Use a signer boundary. The AI may prepare an unsigned event but must not access signing secrets.
5. Publishing observations is a public external side effect. Obtain explicit approval before enabling a crawler publisher or sending an event to relays. First validate and dry-run.
6. SIP-01 carries observations, not search queries. Do not put user search strings or private browsing history in events.
7. Ranking, moderation and trust are engine-local. Do not publish private ranking scores as protocol facts.
8. Distinct pubkeys observing the same d provide an indexer-count signal, not proof that the page is accurate or that the indexers are independent. Sybil resistance is an application problem.
9. NIP-50 operators and the uncaged_index NIP-11 block are optional relay accelerations. Check the actual relay capabilities. Multi-letter extension tags are not portable stock-relay filters.
10. EOSE does not guarantee exhaustive stored results. Use NIP-67 hints where supported; only finish is definitive. If the relay provides no finish hint, report completeness as unknown. If it signals more, paginate or clearly label results incomplete.

## Safe publishing flow

1. Normalize and validate the source URL.
2. Build the unsigned kind 39697 event with scripts/sip01.py.
3. Inspect the event for private URLs, credentials, user query terms and sensitive metadata.
4. Recompute d/x and run selftest.
5. Present the exact event and relay targets to the user.
6. Obtain approval, then sign through the dedicated signer and publish.
7. Record relay acknowledgements separately from proof of global propagation. One or more OK acknowledgements do not establish network-wide availability.

Example build only (no key or signature involved):

~~~bash
python3 scripts/sip01.py build "$URL" "$TITLE" --description "$DESC" --topic nostr --lang en --source mycrawler/1
~~~

The output is unsigned. Do not pipe it into a helper that accepts an nsec argument. Send it to the configured signer through its supported interface.

## Validation before responding

- Validate exactly-one and required-tag rules against the current specification.
- Recompute d and x using the published values.
- Verify event id and signature with a maintained Nostr library; do not implement crypto in this skill.
- Distinguish build success, signer approval, relay OK acknowledgement, indexer agreement and search-result completeness. They are different facts.
- If a result set may be partial, say so explicitly.

## Source provenance

For normative answers, note the canonical SIP-01 revision/date and relevant section. For Nostr dependencies, check the current upstream NIPs repository separately. Mark implementation-specific behavior as such.
