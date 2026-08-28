# Provenance

Every file in this repository is copied verbatim from the private factory repository
(`HS-marketing-skills`) under an explicit allowlist. Nothing here is hand-edited after copy; a
divergence between this tree and the factory's committed records is a defect, not a revision.

| Public path | Factory source | Factory commit |
|-------------|----------------|----------------|
| `specs/protocol-v0.1.md` | `specs/protocol-v0.1.md` | `e5ff057` (last substantive change; pre-registered 2026-07-16) |
| `specs/amendment-D8-local-generation-v0.1.md` | same path | `cc15a80` |
| `fixture/slatebridge.md` | same path | pilot spine (2026-07) |
| `briefs/S1.json` `S2.json` `S3.json` | same paths | pilot spine (2026-07) |
| `benchmark/corpus/manifests/hendry-*.json` (9) | same paths | `cc15a80` |
| `runs/0.1-L1/*.json` (99) | same paths | `7a2cb87` |
| `scores/0.1-L1/*.json` (3) | same paths | `7a2cb87` |
| `LICENSE` | CC BY 4.0, identical to the `ai-marketing-operator-logs` mirror | — |

First assembly 2026-08-28 was a manual allowlist copy, recorded here; subsequent updates are to
come from a deterministic publisher step in the factory's toolchain, which will refuse any path
outside the allowlist. The factory's own public-release gate
(`docs/PUBLIC-RELEASE-BLOCKERS.md`) governs what may ever be added: no competitor skill text, no
named grade without author consent, no journals, no secrets.
