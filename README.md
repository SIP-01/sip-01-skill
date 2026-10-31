# sip-01-skill

Agent skill for [SIP-01](https://github.com/NostrDanish/SIP-01), the Search Index Protocol (Nostr kind 39697, Web Index Observations).

`SKILL.md` is the procedure: event shape, rules that keep indexers byte-compatible, and where to look next.

| Path | Use |
|---|---|
| `scripts/sip01.py` | Normalize a URL, compute `d` and `x`, build an unsigned event, validate one. `selftest` runs spec §13. |
| `references/spec.md` | Canonical spec text (v1.2). |
| `references/implementation-guide.md` | Publisher, consumer, and relay walkthroughs. |
| `references/ecosystem.md` | UNCAGED, Crawlstr, indexstr, Searchstr, kind 16919. |

Signing and relay I/O stay in the [nostr skill](https://github.com/NostrDanish/nostr-skill). Set `NOSTR_SECRET_KEY` and pass `-` to `nostr_key.py sign`. Do not put an indexer secret on the command line.

## Check

```bash
python3 scripts/sip01.py selftest
```
