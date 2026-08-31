# ai-marketing-skills

[![verify](https://github.com/hendrysoong/ai-marketing-skills/actions/workflows/verify.yml/badge.svg)](https://github.com/hendrysoong/ai-marketing-skills/actions/workflows/verify.yml)

**The hendry.ai AI Marketing Skills Benchmark. Protocols, run records, and audit
artifacts, published so anyone can check the numbers.**

Built and run by [Hendry Soong](https://hendry.ai). Reports are published on the
[skills track at hendry.ai](https://hendry.ai/ai-marketing/skills); this repository carries the
evidence behind them.

## The skills

`skills/` carries the skills built against this benchmark, packaged as they ship.

| skill | job | measured |
|---|---|---|
| `hendry-cold-email` | one cold B2B outreach email | 9/9 on the field rubric, dominates the bare model and the placebo |
| `hendry-linkedin-inmail` | one cold LinkedIn InMail | 7/7 on the field rubric, dominates both baselines |

**These are not graded.** No judge model has qualified, so nothing here is crowned. What is measured
is **rule conformance**: `benchmark/rubrics/` holds rubrics derived from the checkable rules that
independent skill authors publish about the same job, each rule recording who stated it. A skill
that satisfies more of the field's own published bar than a baseline does dominates it, and
dominance is a partial order, so two skills can be incomparable and are reported that way.

Rule conformance measures compliance, not persuasion. A fully compliant email can still be dull.
It is a floor and never a ceiling.

## What this is

AI marketing skills, packaged instruction files that a model loads before writing, mostly get
adopted on faith: a skill ships, a thread praises it, nobody measures whether it makes output
better. This benchmark measures that. Every skill is tested against the same fictional company on the
same briefs, in arms that differ only in one variable:

| Arm | What it gets |
|-----|--------------|
| B0, bare model | company facts + brief, nothing else |
| B1, placebo | B0 plus one fixed 12-word expert line |
| S, skill | B0 plus the full skill text, loaded verbatim |

A judge model compares outputs pairwise, blind and order-swapped, and win rates become grades on
bands that were fixed before any data existed. The benchmark measures craft lift only, whether
the skill improves the writing. It makes no claims about conversions, reach, or revenue.

Before any judge grades anything, it must pass a perturbation audit: the same comparisons
re-asked with the grading rubric reversed and the post labels renamed. A judge that changes its
winner when only the names change is refused the seat.

## What is in this repository

| Path | Contents |
|------|----------|
| `specs/` | The operative protocol (v0.1) and the ratified local-lane amendment (protocol 0.1-L1) |
| `fixture/` | Slatebridge, the fictional test company. Every fact is invented and labeled as such |
| `briefs/` | The three briefs every arm answers |
| `benchmark/corpus/manifests/` | The nine hendry.ai skills under test, pinned by content hash |
| `runs/0.1-L1/` | All 99 generation records of the first cycle, append-only, token counts and pinned model strings included |
| `scores/0.1-L1/` | The model digest pins and both judge-audit artifacts of the first cycle |

Current state, as of 28 August 2026: the first 0.1-L1 cycle generated its full 99-post matrix,
then refused to grade it. Both candidate judge models failed the perturbation audit, at 72.2 and
83.3 percent verdict stability against a 90 percent bar. Zero judgments exist, so zero grades
exist, and nothing is crowned. That refusal is the first published result.

## What is deliberately not here

- **Competitor skill texts.** The protocol's pilot evaluated author-released skills by other
  authors. Their text is not rehosted here, and no named grade appears anywhere before the
  author has been contacted, per the protocol's publication rules (see `specs/protocol-v0.1.md`,
  section 11).
- **The hendry.ai skills themselves.** This repository publishes the measurements. The skills
  are pinned here by content hash so any future grade attaches to an exact byte-state.
- **The harness source.** The runner, judge, and audit code live in the private factory for now;
  the records they produce are what this repository exists to publish.

## How to check the numbers

Records are append-only and every figure in a published report cites a record in this tree by
id. Run records carry the exact runtime-returned model string; the audit artifacts carry the
per-pairing verdicts under each prompt variant; skill manifests carry a sha256 over the exact
text the skill arm loaded. If a report says a number this tree cannot reproduce, the report is
wrong; say so.

CI enforces this on every push and pull request (`tools/verify.mjs`, no dependencies, no
network):

1. The pre-registered protocol, the ratified amendment, and the fixture are byte-pinned by
   sha256. Any edit fails the build; changes only ever arrive as new amendment files.
2. The 99-run matrix must be complete, every record well-formed, and every skill run bound by
   hash to its pinned manifest.
3. The judge-audit stabilities are recomputed from their own per-pairing verdicts. If 72.2 and
   83.3 percent stop re-deriving, the build fails.
4. A grade file without judgments behind it fails the build. Zero judgments exist today, so
   zero grades exist today.
5. Pull requests may only add records. Modifying or deleting an existing record, manifest, or
   the protocol fails the build.

You can run the same check locally: `node tools/verify.mjs`.

The private factory that produces these records runs its own harness: a 141-check verification
suite, a network tripwire that kills the process on any socket outside the pinned local
runtime, append-only atomic writers, and pre-registered thresholds that never move after data.
This repository is that harness's public face.

## Names

The public name is the hendry.ai AI Marketing Skills Benchmark, and this repository uses the
descriptive name so both humans and agents can tell at a glance what it holds. SKILLSMASH is
the benchmark's internal codename; it appears inside the synced protocol and record files,
which are append-only history and are never rewritten. The winning hendry.ai skills ship as
products under [jtbd.ai](https://jtbd.ai); this repository is the evidence, never the store.

## License

Contents are published under CC BY 4.0 (see `LICENSE`). Attribution: Hendry Soong, hendry.ai.
