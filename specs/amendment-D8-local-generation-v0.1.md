# Amendment D8 — Local Generation as a New Protocol Version (0.1-L1)

**Date:** 2026-08-27 (revision b, same day — incorporating the 47-agent cross-agent review; see §11)
· **Status:** direction ratified by the owner 2026-08-27 (decision panel, prompt-0029 session:
"Local generation — D8 amendment"); the parameter set below is **pre-registered before any 0.1-L1
datum exists** and binds at the commit gate when the owner approves this file's staged diff.
· **Scope:** execution plane + protocol lineage.
· **Supersedes:** **amendment-M0 §1 (mode table) and §2 (socket preflight), narrowly and only as §9
below states.** Nothing else: protocol-v0.1.md stays the operative protocol for every v0.1 claim;
0.1-L1 is a sibling version with its own baseline, never a swap inside v0.1.

## Problem being amended

Generation does not exist: `--live` throws `E_PHASE_1B_LOCKED` (`src/safety/live-permit.ts:25`), so
the instrument that produced the one real run (2026-07-18) cannot run. Live Phase 1B is a governance
gate on paid provider calls; local inference sidesteps the thing the gate exists to control (spend,
credentials, external transport). But a local generator changes what `WR_B0` means — "beats the bare
baseline" becomes "beats the bare **local** model", a weaker claim — so local execution is a **new
protocol version with its own baseline**, per the recorded rule (lab note
`docs/decisions/skills-iteration-loop.md` §3; carried into prompt-0029 §1.1).

## 1. Version and comparability

- **Protocol version string: `0.1-L1`.** Every run, judgment, and score record carries it.
- **`WR_B0` under 0.1-L1 reads "beats the bare local model."** It is not the v0.1 claim.
- **`WR_B1` under 0.1-L1 reads "beats the placebo line on the bare local model."** The placebo text
  is v0.1's verbatim; the generator under it is not. Every WR sentence names its baseline **and**
  its generator class.
- **No 0.1-L1 number OR GRADE LETTER is ever quoted beside a v0.1 number or grade.** A 0.1-L1
  grade is always written with its version — "F (0.1-L1)" — and never appears in a table, sentence,
  or chart with a v0.1 grade. The 2026-07-18 pilot grades stay v0.1 facts.
- **The local baseline is a lower bar and every report says so.** Beating a bare 7B-class model is
  systematically easier than beating the bare frontier model v0.1 used; that is exactly why the
  grades are version-scoped and the two lineages never co-quote.
- Adopting 0.1-L1 does not retire v0.1. A future live Phase 1B run would resume v0.1's lineage.

## 2. What moves, what stays (delta vs protocol-v0.1.md)

| Parameter | v0.1 (operative) | 0.1-L1 (this amendment) |
|---|---|---|
| Generator | `claude-sonnet-4-6`, temp 1.0, max 1024 | **`qwen2.5:7b-instruct-q4_K_M` via a loopback Ollama runtime**, temp 1.0, max 1024, unseeded (carrying v0.1's unseeded design), sampling pinned per §3 |
| Judge | `claude-sonnet-4-6`, temp 0 (same family as generator — stated limitation, §4) | **The first seat in `[llama3.1:8b-instruct-q4_K_M, gemma2:9b-instruct-q4_K_M]` to pass the §4 perturbation audit**, temp 0, **max 512 output tokens**, JSON output constrained by the runtime's `format: json` grammar. Disjoint family from the generator in either case (Meta / Google vs Alibaba), closing v0.1's stated same-family limitation — **and opening a new one, disclosed in §4** |
| Transport | paid provider API (Phase 1B locked) | **loopback only** (`127.0.0.1`, pinned port 11434). External network stays denied; no credential is read (§9) |
| Baseline B0 | bare pinned frontier model | bare pinned **local** model |
| Placebo B1 | fixed 12-word line | **verbatim identical** |
| Briefs, fixture | S1–S3, Slatebridge | **verbatim identical** |
| Run matrix | pilot cohort per §5 | **Enumerated:** arms = B0, B1, and exactly the nine §5 skills; 11 conditions × 3 briefs × 3 runs = **99 generations**. Baseline pairings only — skill vs B0 and skill vs B1, run-matched, order-swapped = **at most 324 judge calls**. **Head-to-head pairings are excluded from this cycle entirely** (§6.6) |
| Judge prompt | protocol §6, verbatim | **verbatim identical** (the §4 audit variants exist only inside the audit and never judge a scored pairing) |
| Metric formulas, grade bands | §7, §8 | **verbatim identical, re-affirmed in §6.1** |
| Headline metric | §7 names `WR_B1` the headline | **EXPLICIT DELTA: the 0.1-L1 headline is `WR_B0`** (§6.2 states why). The formulas carry verbatim; the headline designation does not |
| Reliability flags | §7: consistency < 70% flags "unreliable"; §8: consistency < 60% caps the grade at C | **both carried verbatim** — the 70% flag and the 60% cap are distinct rules and both apply |
| Adoption rule | §10 | **identical**, read against 0.1-L1's own baseline; adoption remains a private-registry decision and §6.4's no-crowning rule governs everything published |
| Gate semantics | hard gate ⇒ auto-loss, judge call skipped | **identical.** A15 is NOT ratified by this amendment. The 0.1-L1 hard-gate set is exactly v0.1's as implemented in `src/gates.ts`: `word_cap`, `hashtag_cap`, `ending_shape`, `tone_exclamation`, `tone_banned_word` (`ai_cliche` and `fact_containment` are review-severity and never auto-lose). **Gate-decided share** = synthetic gate judgments ÷ all judgment calls for that skill, reported per skill (§6.5) |

## 3. Model identity and sampling (pre-registered)

- **Tags pin quantization explicitly** (q4_K_M) — no floating "latest" resolution.
- **Digest pinning:** before the first model call of the cycle, the runtime digest of the generator
  and of every judge seat is recorded append-only in `scores/0.1-L1/_models.json`. Every later
  stage re-resolves and compares; **a mid-cycle digest change aborts the stage.**
- **Sampling parameters are pinned, never left to runtime or model-card defaults:**
  `top_p 0.9, top_k 40, repeat_penalty 1.1, num_ctx 16384`, set explicitly on every call. `num_ctx`
  is pinned above the largest assembled prompt — the runtime default (4096) would silently truncate
  the ~4.4k-token skills, which the cross-agent review caught before it could corrupt a single run.

## 4. Judge seat lock: the perturbation audit (pre-registered)

Canon's standing rule (v0.3 §10.3 / A10): never trust order-swap alone; perturbation-test at every
judge lock and seat change — and an 8B judge is precisely the class canon records as most fragile.
So **no judging happens until a seat passes this audit**:

- **Material:** the frozen v0.1 pilot texts — `corey-social` vs `b0` and vs `b1`, briefs S1–S3,
  run-matched r1–r3, presentation order 1 = **18 pairings**. Audit-only: these verdicts are never
  scored, and the material predates every 0.1-L1 record.
- **Variants (exactly three, registered in `src/local-cycle.ts` `auditVariants()`):**
  V1 the operative prompt verbatim; V2 the five rubric criteria in reversed order, nothing else
  moved; V3 the post labels renamed A/B → X/Y everywhere, nothing else moved.
- **Stability:** a pairing is stable when all three variants return the identical winner and none
  is unparseable. **Pass = stability ≥ 90% over the 18 pairings.**
- **Seats, in pre-registered order:** `llama3.1:8b-instruct-q4_K_M`, then
  `gemma2:9b-instruct-q4_K_M`. The first passing seat judges the cycle and the lock is recorded
  append-only (`_judge-seat.json`, with the full per-pairing audit in `_judge-audit-*.json`).
  **Both failing halts the cycle for the owner — no third seat is improvised.**
- **Disclosed limitation, carried wherever 0.1-L1 results appear:** the judge is a single small
  local model. The audit and order-swapping mitigate prompt fragility; they do not equal v0.1's
  frontier judge, nor the v0.2 three-family panel. The swap fixes the same-family defect and
  introduces a weaker-judge defect; both sentences appear together.

## 5. The nine own skills enter the corpus (append-only, content-pinned)

`skills/<name>/` is copied **verbatim** into `benchmark/corpus/raw/<slug>/`
(`scripts/sync-own-skills.mjs` refuses any later divergence byte-for-byte). Declared Load Set rule
is v0.1's Q2 rule unchanged; adjudications: `anti-slop.md` **included** where present (bound as "a
floor, not a suggestion" over every line); `agents/openai.yaml` **excluded** everywhere (platform
packaging). Manifest metadata: author Hendry Soong, provenance `author-released`, species `type-1`,
license `proprietary`, `source_ref` `0a1a656d06e903ec0b5d1839359c4769b5106ca0`. Ingest is
append-only; the three frozen v0.1 manifests are untouched.

**The subjects are pinned by content hash, not by name** (sha256 over the Declared Load Set,
estimated tokens beside each; grades attach to these hashes):

| Slug | jtbd_claim | version_hash | tokens (est.) |
|---|---|---|---|
| hendry-comment-lean | li-comment | `ab1bec7f7dbe2b420fd19957b1ba54cb85ef2d22babf5b4ce55a416e65c4a868` | 1065 |
| hendry-comment-recipe | li-comment | `d5287ad81cab11857736d0befe821fdd85d28bb1288de30f645c3b74bff80920` | 1418 |
| hendry-email-webinar-invite | email-webinar | `dfe72976b6d7a648323912d010b20c49e86daff3fa41d61004a57816aa9dba9f` | 3682 |
| hendry-email-webinar-reminder | email-webinar | `03cbec9b711736c89b53bedcd33989cfc8f5688aa0b350588105fcfcc8842ace` | 4378 |
| hendry-linkedin-post-expert | li-full-post | `d4d2e4db29388a0742f125fbbece9833f7ac296ab00268a29e1558fb798d5ee6` | 3897 |
| hendry-linkedin-post-narrative | li-full-post | `88279cfd0e5e70e6994eeb56ba900a03b99bd5d38e2aa64e791381312d36f5c5` | 3809 |
| hendry-post-exemplar | li-full-post | `84758ae4406b216092f8277719bab8ce9693abf23f1ff74b5e76aac40dcd3ab7` | 2987 |
| hendry-post-lean | li-full-post | `b924ab55b185ecacf1813c77f38d3c910c21697d9bb0dd1792c3f778c73bd9fe` | 1754 |
| hendry-post-structured | li-full-post | `9accb7822de883cd274295f45e6c0b6149de3f3d281ab6a950f1f7635c3e5882` | 2488 |

**Off-category rule (Q1 amendment, carried and made operational):** the four non-`li-full-post`
skills run on S1–S3 off-category. Their metrics compute mechanically and their records are kept,
but in any report they appear **only in a separate "off-category — exhibited, not graded" section**:
never pooled with on-category skills, never ranked, never quoted as a verdict on the skill's own
job. The flag is `off-category` on every affected record.

## 6. Metrics, intervals, and reporting rules (pre-registered)

1. **Formulas and bands verbatim from v0.1 §7–§8, no band moved:** gate first — pooled `WR_B0`
   below 50% is grade F regardless of anything else; otherwise by pooled `WR_B1`: A ≥ 70%,
   B 60–69.9%, C 50–59.9%, D < 50%. Consistency < 60% caps at C; consistency < 70% flags
   "unreliable". These bind before any 0.1-L1 datum exists and never move after data.
2. **Headline `WR_B0`, and say why** (an explicit delta from v0.1's headline designation, declared
   in §2): the v0.2 headline (`WR_B1`/`WR_B2`) is unquotable while the B2 strong placebo is a
   literal placeholder (`specs/data-schema-v0.2.md:298`), and `WR_B0 ≥ 0.5` is the floor that
   decided every pilot grade. `WR_B1` is reported second, baseline-named per §1. No `WR_B2`
   appears anywhere.
3. **No point estimate without its interval** (v0.3 §10.11): every reported WR carries a seeded
   percentile-bootstrap 95% CI (seed 42, 10,000 resamples, outcomes win=1/tie=0.5/loss=0),
   computed and stored in the score records. At this matrix size the intervals are wide; reporting
   them wide is the point.
4. **Nothing is crowned — while ground truth is unqualified, regardless of cycle count.** Rater
   self-consistency 3/6 and qualification 6/12-at-chance stand; until a qualification re-run
   passes, no champion, no "best", no public adoption announcement — not from this cycle, not from
   accumulated cycles. Benchmark outcomes only.
5. **The gate-decided share is reported per skill** (§2 definition). The pilot's lesson stands:
   identical win rates across unrelated skills mean a gate, not craft, decided — the report checks
   for and states this pattern.
6. **Head-to-head: excluded.** No head-to-head pairing is judged in this cycle and no
   skill-vs-skill claim of any kind is made. "My skill beats X" has no data behind it under this
   amendment and may not be written. A future head-to-head cycle needs its own pre-registration.
7. **Transcript audit before any report** (A12-style, v0.3 §10.7): at least 10 judge rationales —
   selected by the pinned seed, plus **every** gate-fail and every unparseable call — are read; the
   audit note is committed beside the cycle manifest and the report links it. No headline WR stands
   unaudited.
8. **Held-out disclosure, stated wherever 0.1-L1 results appear:** the skills' author has had
   briefs S1–S3, the fixture, and the judge prompt in-repo since July 2026, and the skills were
   authored in that window. S1–S3 are therefore **not held-out** with respect to the nine
   subjects. This cycle is a first calibration measurement, not held-out craft-lift evidence;
   held-out claims require fresh briefs under A8/A13 machinery.
9. **Publication naming (v0.1 §11 adapted for self-owned subjects):** all nine are published
   **with names regardless of grade** — the owner is the author and consents uniformly, so the
   name-the-strong/anonymize-the-weak split cannot become cherry-picking here. Every figure traces
   to a committed run/judgment/score artifact by id.

## 7. Records, provenance, and the writer (E27 wired)

- 0.1-L1 generations are **`confirmatory`**: a real governed generation — fresh context per call,
  concrete runtime-returned model string, positive token counts from the runtime — through the E27
  governed writer (`src/export.ts`), append-only into **version-scoped subtrees `runs/0.1-L1/` and
  `judgments/0.1-L1/`**. The flat v0.1 trees are never touched; run ids keep the v0.1 convention
  and are unique within their protocol version (the pilot owns `s1-b0-r1` etc. in the flat tree).
  The writer's anti-laundering guards stand unchanged.
- **Completion, never rerun:** the cycle fills only missing (brief, arm, run_index) triples. An
  existing record is never overwritten; a true rerun is a new run id.
- **Generation failure halts the stage loudly** — a transport error, empty output, or non-positive
  token count writes nothing and stops the loop; re-invocation is manual and the halt is recorded
  in the session log. **Stated limitation:** each record persists immediately after its own
  generation, so a crash between a generation and its persist loses at most that one sample, whose
  re-generation on resume is a fresh draw. The window is one record wide; the design accepts and
  discloses it rather than pretending a ledger exists.
- Scores are derived data under **`scores/0.1-L1/`** (frozen v0.1 `scores/*.json` untouched), plus
  the cycle manifest (`_cycle.json`) recording model digests, sampling, counts, the gate-decided
  and unparseable tallies, and the grading-validity verdict.

## 8. Judge-call failure rules (pre-registered)

- With `format: json`, a response must **parse as one JSON object AND conform to the registered
  schema** — `winner` among the two presented labels or `"tie"`, integer `margin` 1..3, `reasons`
  an array. Valid JSON in any other shape is unparseable; there is no salvage parsing and no
  hand-repair, ever.
- One retry with the identical prompt; still unparseable, the **call** records as a tie with reason
  `judge-unparseable`.
- **Validity floor:** if unparseable calls exceed **5% of all judge calls**, the cycle is
  **invalid for grading** — the scorer refuses to write scores, the cycle manifest says
  `cycle_valid_for_grading: false`, and a report may describe the failure but may quote no grades.

## 9. Execution safety — the explicit M0 delta

Amendment M0's mode table (§1) closes the mode set with every non-live mode's network column
"denied", and its §2 preflight requires a literal `--live` before opening any socket. **This
amendment supersedes those two clauses, narrowly, as follows** — everything else in M0 carries in
full force, and per M0's own preamble this widening is stated here for the owner's explicit
ratification rather than smuggled:

- **Mode table, new row:** `--local` — filesystem: version-scoped record subtrees only
  (`runs/0.1-L1`, `judgments/0.1-L1`, `scores/0.1-L1`, `benchmark/corpus/manifests` append-only)
  plus approved scratch; network: **loopback `127.0.0.1` to pinned port 11434 only**; credentials:
  none read; subprocesses/workers: denied; everything else: denied.
- **Socket preflight:** a socket may open without `--live` **only** under literal `--local`, only
  to `127.0.0.1:11434`, only from the sanctioned provider module
  (`src/safety/local-provider.ts`, granted `node:http` for exactly that file in the static
  boundary check), and only under the loopback tripwire (`scripts/local-preload.cjs`), which keeps
  https/http2/tls/dns/dgram/fetch/websockets/subprocesses/workers/listen and every non-loopback
  `net` target denied and fails the process (exit 86) on any attempt.
- **Unchanged:** Phase 1B stays locked (`--live` still throws `E_PHASE_1B_LOCKED`); the offline
  default stays fail-closed; M0's acceptance canaries for the offline preload are untouched — the
  loopback allowance exists only in the local preload, which no legacy CLI loads.
- The local runtime (Ollama) and the pinned models are installed with the owner's explicit
  download approval, recorded in the session log with sources and sizes.

## 10. What this amendment does not do

It does not ratify A15 (gate taxonomy), does not touch the human-calibration tier (the honest
blocker on crowning stays where it is), does not add B2, does not alter any v0.1 threshold, band,
brief, fixture, or judge-prompt text, does not permit any head-to-head claim, and does not open
any external network path.

## 11. Cross-agent review disposition (2026-08-27)

Per the standing rule ("cross-agent review the design; self-review missed all of it"), revision a
of this amendment went through a 47-agent adversarial review (4 lenses × find, then per-finding
skeptic verification) before ratification. **27 findings confirmed, 16 refuted.** Every confirmed
finding is incorporated in revision b: the M0 supersession is now explicit (§9); the judge seat is
audit-locked with the single-small-judge limitation disclosed (§4); the headline reassignment is a
declared delta (§2, §6.2); sampling, `num_ctx`, digests, quantization, judge output cap, the
schema-strict parse rule, the unparseable validity floor, the generation-failure rule, the run
matrix, the gate set and gate-decided denominator, both reliability thresholds, subject content
hashes, version-scoped grade letters, `WR_B1` redefinition, the crowning-ban rescope, off-category
reporting, head-to-head exclusion, CIs, the transcript audit, the held-out disclosure, and the
uniform-naming publication rule are all pinned above. The refuted findings and full verdicts live
in the lab session record (workflow `wf_3b7a48dd-41d`).
