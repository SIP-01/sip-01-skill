# sip-01-skill

Agent skill for [SIP-01](https://github.com/NostrDanish/SIP-01) — the Search Index Protocol. One addressable Nostr event (kind **39697**) per `(indexer pubkey, normalized URL)`.

Crawlers publish, relays store/validate/index, engines group-and-rank. This skill stays on **event construction and validation**. Signing and relay I/O belong to [nostr-skill](https://github.com/NostrDanish/nostr-skill).

## Layout

```
SKILL.md                          event shape, rules, task routing
scripts/sip01.py                  zero-dep normalize / d / x / build / validate / selftest
references/spec.md                canonical spec v1.2 (verbatim)
references/implementation-guide.md publisher / consumer / relay walkthroughs
references/ecosystem.md           UNCAGED / Crawlstr / NIP-50 operators / kind 16919
```

## Quick start

```bash
python3 scripts/sip01.py selftest
python3 scripts/sip01.py normalize 'https://Example.com/Path/?b=2&a=1'
python3 scripts/sip01.py build 'https://example.com/page' 'Example Page' --description '…' --topic nostr --lang en
```

Publish (after building): sign with `nostr-skill` `scripts/nostr_key.py sign`, then `scripts/nostr_query.py publish`.

## Spec freshness

`references/spec.md` is a pinned copy of SIP-01 **v1.2**. When changing protocol behavior, compare the header version to upstream `https://github.com/NostrDanish/SIP-01` before editing.

## License

CC-BY-SA-4.0
