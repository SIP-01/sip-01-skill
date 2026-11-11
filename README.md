# SIP-01 skill

Portable Agent Skill package for the SIP-01 draft Search Index Protocol (kind 39697).

## Install

Clone into a skills directory named exactly `sip-01` (the folder must match frontmatter):

~~~bash
git clone https://github.com/SIP-01/sip-01-skill <skills-root>/sip-01
~~~

Requires Python 3.10+ only for the zero-dependency URL/hash/event helper. This helper does not sign events or handle secret keys.

## Contents

- `SKILL.md`: activation, safe workflow, key-custody and completeness rules.
- `references/spec.md`: snapshot of the canonical v1.3 spec.
- `references/implementation-guide.md`: publishing, consuming and relay integration.
- `references/ecosystem.md`: ecosystem notes.
- `scripts/sip01.py`: normalization, d/x hashing, event build/validate and spec-vector selftest.

## Verify

~~~bash
python3 scripts/sip01.py selftest
python3 scripts/sip01.py build https://example.com Example
~~~

The build command emits an unsigned event. Signing and publishing are separate privileged steps performed by a dedicated signer after explicit approval. Do not pass an nsec to a shell command or agent.
