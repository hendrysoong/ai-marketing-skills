# Eval Protocol v0.1

Working name: SKILLSMASH (placeholder, find and replace when named). Status: draft for first run. This document gets published with the first results.

## 1. What this measures, and what it does not

This benchmark measures craft lift: whether loading a skill produces better output than the same model without it, judged blind against a published rubric. It does not measure business outcomes. No claim here is a claim about conversions, reach, or revenue. That limitation is stated wherever results appear.

## 2. Test subject and category

All skills are tested against one synthetic brand, Slatebridge (see fixture-brief.md). The fixture is fictional, neutral, and published as part of this protocol so anyone can reproduce a run.

v0.1 category: social content (LinkedIn posts). One category per protocol version. New categories get their own briefs and rubric.

## 3. Conditions

Every condition receives the identical brief and the identical fixture facts. The only variable is the skill.

- **B0, vanilla**: model + fixture facts + brief. Nothing else.
- **B1, placebo**: B0 plus one fixed line: "You are an expert B2B social media copywriter. Apply best practices." This line never changes across runs or skills.
- **S, skill**: B0 plus the full skill text, loaded verbatim.

B1 exists to expose bloat. A skill that cannot beat a 12-word expert framing is not encoding knowledge.

## 4. Models and parameters

- Generator: claude-sonnet-4-6, temperature 1.0, fixed max tokens. Exact version string recorded on every run.
- Judge: claude-sonnet-4-6, temperature 0.
- Known limitation, stated inline: the judge shares a model family with the generator. Mitigations in v0.1: the placebo arm, order-swapped pairwise judging, and identical judge treatment for all conditions. v0.2 adds a second judge from a different model family and human calibration votes.

## 5. Run matrix (pilot cohort)

3 skills (Corey Haines, Diandra Escobar adapted, Eric Siu) + 2 baselines, 3 briefs, 3 runs per condition per brief.

- Generations: 5 conditions × 3 briefs × 3 runs = 45
- Baseline judgments: each skill vs B0 and vs B1, run-matched pairings (run i vs run i), each pairing judged twice with order swapped = 3 skills × 2 baselines × 3 briefs × 3 pairings × 2 orders = 108 calls
- Head-to-head judgments: 3 skill pairs × 3 briefs × 3 pairings × 2 orders = 54 calls
- Total: 45 generations, 162 judge calls. Single-digit dollars on the pinned model.

## 6. Judge design

Pairwise, blind, order-swapped. The judge never sees condition labels, author names, or skill names. Each pairing is judged twice (A-B, then B-A). If the two verdicts disagree, the pairing scores as a tie.

Judge prompt v0.1:

> You are judging two LinkedIn posts written from the same brief for the same company. You will receive the brief, the company facts, Post A, and Post B.
>
> Decide which post better serves the brief for the stated audience. Weigh: (1) hook strength in the first two lines, (2) fit to the audience's real working context, (3) specificity: concrete facts used well, no invented claims, (4) structural craft: rhythm, scannability, a clear next step, (5) adherence to the brief's constraints.
>
> Do not reward generic polish over concrete usefulness. Do not reward length. A post that invents facts not in the company materials loses on that basis.
>
> Respond with JSON only: {"winner": "A" | "B" | "tie", "margin": 1 to 3, "reasons": ["...", "...", "..."]}

## 7. Metrics

- **WR_B0**: win rate vs vanilla. Wins / (wins + losses), ties count 0.5. Reported per brief and pooled.
- **WR_B1**: win rate vs placebo. Same formula. This is the headline number.
- **Consistency**: share of a skill's judge calls that agree with its majority outcome, pooled. Below 70% gets flagged "unreliable".
- **Lift per 1k tokens**: (WR_B1 minus 50) divided by (skill prompt tokens / 1000). Rewards skills that earn their context window.

## 8. Grades

Gate first: pooled WR_B0 below 50% means the skill makes output worse than nothing. Grade F, regardless of anything else.

Otherwise, by pooled WR_B1:

- A: 70% or above
- B: 60 to 69.9%
- C: 50 to 59.9% (no reliable lift over a one-line framing)
- D: below 50% (beats nothing, loses to the placebo)

Consistency below 60% caps the grade at C and adds the unreliable flag.

## 9. Provenance and species

Every skill carries two labels on its report card:

- Provenance: **author-released** (published by the author as a skill) or **community-adapted** (reconstructed from the author's public material; the grade applies to the adaptation, not the author).
- Species: **Type I** (instruction skill, markdown) or **Type II** (workflow skill, code and scripts).

v0.1 scope is Type I only. For Eric Siu's repo, select his most instruction-style skill. Type II needs sandboxed execution and gets its own protocol in v0.2.

## 10. Adoption rule

The answer to "is this worth adding to my stack" is a checklist, not a feeling. A skill is ADOPTED when all of the following hold:

1. Pooled WR_B1 at or above 65%, and no single brief below 50%.
2. Consistency at or above 70%.
3. Survives the context re-test: re-run against my own brand context with WR_B1 at or above 60% and zero voice or governance violations in outputs.
4. Cost sanity: skill under 6k tokens, or lift per 1k tokens justifies the size.

WATCH: pooled WR_B1 between 55 and 64.9%. Re-test on the next harness model version.
SKIP: everything else.

Adopted skills enter the private registry with grade, provenance, version hash, harness model, and test date. Every adopted skill is re-run when the harness model changes.

## 11. Publication rules

1. Authors are contacted before any named grade is published. Community-adapted skills require the author to see the adaptation first.
2. Weak results are published as aggregate statistics without names. Strong results are published with names, with consent.
3. This protocol, the judge prompt, the rubric, the fixture, and all run data are published with the results.
4. Fictional fixture facts are labeled fictional everywhere they appear.

## 12. Reproducibility

Model version strings, parameters, prompts, run outputs, and judgments are committed to the repo. Anyone can re-run the benchmark and check the grades.
