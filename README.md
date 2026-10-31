# sip-01-skill

Agent skill for **SIP-01** (Search Index Protocol): kind **39697** web index
observations. Spec snapshot in this repo is **v1.2**. Upstream:
https://github.com/NostrDanish/SIP-01 — refresh `references/spec.md` when
that repo releases.

Signing and relay I/O are delegated to
[nostr-skill](https://github.com/NostrDanish/nostr-skill)
(`nostr_key.py sign`, `nostr_query.py publish`).

## Layout

```
SKILL.md                         event shape, routing, rules
scripts/sip01.py                 normalize / build / validate / selftest
references/spec.md               spec v1.2 snapshot
references/implementation-guide.md
references/ecosystem.md          UNCAGED / Crawlstr / indexstr / NIP-50
```

## Test

```bash
python3 scripts/sip01.py selftest
```

The upstream crawler name is sometimes spelled **Crwalstr** in repo titles
(historical typo). Treat "Crawlstr" and "Crwalstr" as the same project.

## Install

Copy or symlink into the agent's skills directory. Pair with `nostr`.
