# sip-01-skill

Agent skill for [SIP-01](https://github.com/NostrDanish/SIP-01), the Search Index Protocol: Nostr kind **39697** web-index observations.

[`SKILL.md`](SKILL.md) is the routing page. The canonical spec and the implementation guide are copied under `references/` and should stay verbatim. `scripts/sip01.py` is the zero-dependency normalizer, builder, and validator.

```bash
python3 scripts/sip01.py selftest    # spec test vectors
python3 scripts/sip01.py validate '<event-json>'
```

Signing and relay I/O stay in the [nostr skill](https://github.com/NostrDanish/nostr-skill). Set `NOSTR_SECRET_KEY` and pipe the unsigned event into `nostr_key.py sign -`. The indexer key is a separate pseudonymous key, not a person's nsec.

## Install

Copy or symlink the directory that contains `SKILL.md` into the agent's skills path.
