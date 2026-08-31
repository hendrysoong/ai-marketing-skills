# Amendment D8 — Local Generation as a New Protocol Version (0.1-L1)

**Date:** 2026-08-27 (revision b, same day — incorporating the 47-agent cross-agent review; see §11)
· 2026-08-28 (revision c — judge seats 3–4, audit reason persistence, exhibit rules; see §12,
pre-registered before any seat-3/4 datum exists)
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
- **Digest timing for seats that have not yet audited: refined by revision c (§12.3).**

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
  **That halt happened on 2026-08-27 and the owner answered it: the seat list is extended by
  revision c (§12.1) with seats 3–4, in order; an audited seat is never re-audited.**
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

## 12. Revision c — seats 3–4, audit reason persistence, exhibit rules (2026-08-28)

**Status:** direction ratified by the owner 2026-08-28 (prompt-0030 part-two handover: "ratify
judge seats 3–4 and fix the audit to persist judge reasons"; host decision same session: the cycle
resumes on the Mac mini, not the Air). The parameters below are **pre-registered before any
seat-3/4 datum exists** — neither candidate model is downloaded at ratification time — and bind at
the commit gate when the owner approves this file's staged diff. Everything not named here carries
verbatim from revisions a/b.

### 12.1 Judge seat list extension (§4 delta)

§4's halt rule operated as designed on 2026-08-27: both seats refused, zero judgments, the cycle
stopped at the owner gate. The owner's answer extends the pre-registered seat order with exactly
two further candidates, audited in order:

3. `phi4:14b-q4_K_M` (Microsoft Phi-4, 14B — a class above the failed 8–9B seats)
4. `mistral-nemo:12b-instruct-2407-q4_K_M` (Mistral AI, 12B)

Family disjointness is preserved and now spans five houses: generator Alibaba; seats Meta, Google,
Microsoft, Mistral — no family repeats, and neither candidate shares a family with the generator.
The audit material, the three registered variants, the stability definition, and the **90% bar
carry verbatim** from §4. **An audited seat is never re-audited:** the audit stage skips any seat
whose `_judge-audit-*.json` artifact already exists — the 2026-08-27 artifacts for seats 1–2 stand
as recorded, append-only. **All four failing halts the cycle for the owner again — no fifth seat
is improvised**; that outcome is itself the report (three consecutive refusals), and the lane
decision it forces (bigger local model vs paid Phase 1B) is the owner's alone.

### 12.2 The audit persists the judge's stated reasons (§4 harness gap)

The gap was exposed by Report 001's flip exhibit: the 2026-08-27 audit artifacts record winners
only, so the exhibit could show THAT the verdict flipped on a label rename but not what the judge
said it was judging. From seats 3 onward, every audit call persists its full registered verdict
object per variant:

- `verdicts` keeps its existing shape (canonical winner per variant, or `unparseable`) — the
  stability computation is unchanged (winner identity across the three variants).
- A new `details` map records, per variant, the parsed `{winner, margin, reasons}` exactly as
  extracted under the §8 schema rule (reasons capped at 3 as registered), or `null` for an
  unparseable call. No salvage, no paraphrase: the reasons array is the judge's verbatim strings.

Existing audit artifacts are never rewritten. A future flip exhibit shows the reasons beside the
flip, from the record, by pairing id.

### 12.3 Digest pinning for not-yet-audited seats (§3 timing refinement)

§3 pins every model digest before the first call of the cycle — written when all pinned models
preceded the cycle on disk. Under the §12.1 extension, a later seat that never audits (because an
earlier seat locked) need never be downloaded at all. Refined timing, same guarantee:

- The digests of the models the cycle actively uses — the generator, and the locked judge seat
  once one exists — re-resolve and compare at every stage entry; **any change still aborts the
  stage.**
- A not-yet-recorded seat's digest is recorded append-only into `_models.json` immediately before
  that seat's own first audit call. A seat that never audits records nothing and constrains
  nothing.
- A **refused** seat's recorded digest is a historical record: it stays in `_models.json` forever
  but is not re-resolved at later stages, so a migrated host (the Mac mini) need not carry the
  refused models at all.

### 12.4 Exhibit rules (§6 extension: the human reading layer)

The 2026-08-28 owner panel ratified the Report 001 exhibit selection rule before any output text
was read, and the evening session executed it ad hoc. This section indexes that rule properly and
pre-registers Report 002's, and the deterministic generator that makes both reproducible.

- **Report 001 rule (ratified and executed 2026-08-28, recorded here for the index):** specimens =
  brief S1, draw r1, every on-category arm (B0, B1, and the five `li-full-post` skills), presented
  B0, B1, then alphabetical by slug; plus the flip exhibit — the first unstable pairing, in audit
  order, from the first refused seat's audit artifact. Competitor-derived material ships with the
  pilot-skill arm anonymized pending author consent (protocol §11.2 shape).
- **Report 002 per-claim rule (pre-registered before any 0.1-L1 judgment exists):** every headline
  number carries at least one pairing a reader can check. Default selection: for the
  **best-scoring and the worst-scoring on-category skill** by the headline metric (pooled `WR_B0`
  point estimate; ties broken alphabetically by slug), the first pairing in the pre-registered
  order (S1 r1, S1 r2, S1 r3, S2 r1, … S3 r3) of that skill vs the headline baseline B0 whose
  order-1 judgment was a **real judge call** — a synthetic gate verdict does not qualify; if every
  pairing for that skill is gate-decided, the first pairing in that order is shown with its gate
  verdict and said to be one. The exhibit shows both posts verbatim by run id — the skill entry
  leads, the baseline follows — with the canonical verdict, the margin, and the judge's stated
  reasons from the judgment record. **Selection never moves after scores are seen.**
- **The deterministic generator is the only sanctioned emitter.** `src/exhibit.ts`
  (`node scripts/run-local.mjs exhibits --local <rule>`; the npm script surface stays the exact
  M0 allow-list) reads committed records only — runs, judgments, scores, audit artifacts —
  makes no model call, and writes derived exhibit JSON under `scores/0.1-L1/` (derived data, §7
  posture: delete and recompute). Reports embed the generator's `entries` verbatim — paragraph
  structure included — and never hand-build or paraphrase a specimen. The registered text
  conversion: paragraphs split on newlines (blank lines collapse), each line trimmed, empty lines
  dropped, `**bold**` pairs become bold runs, every other byte carried verbatim (an unpaired `**`
  stays literal). The evaluator writes the numbers; the generator writes the specimens; articles
  only quote them.

## 13. Revision d — the order-swap variant joins the qualification audit (2026-08-29)

**Status: RATIFIED by the owner 2026-08-29**, after the machinery was built, tested and pushed
(`7b4e020`) and before any seat had run under it. Everything below was **pre-registered before any
datum produced under it exists**, and it now binds. The owner ratified in the same exchange in
which he was told, explicitly, to expect it to disqualify the seat locked earlier the same day. It is written in the same session, and committed alongside, the transcript audit that
motivated it (`scores/0.1-L1/_transcript-audit.md`), so the record shows the fix was specified
before it was ever run. Everything not named here carries verbatim from revisions a/b/c.

### 13.1 The gap this closes

§4's audit presents its 18 frozen pairings at **presentation order 1 only** and varies the rubric
order (V2) and the post labels (V3). Scoring, by contrast, judges every pairing in **both**
presentation orders. The gate therefore never tested the axis on which a seat can most easily fail.

That gap is now measured, not hypothesised. The seat locked on 2026-08-29,
`mistral-nemo:12b-instruct-2407-q4_K_M`, passed the three-variant audit at 18/18 and then, across
the 109 real-call pairings of the same cycle, disagreed with itself on 90 when the presentation
order was swapped. In **90 of those 90** it selected whichever post was presented first, and in
none did it select the second. A judge can hold every rubric and label perturbation perfectly and
still be answering a question about position rather than about writing.

### 13.2 V4: the fourth registered variant

- **V4 (order-swapped):** the operative judge prompt verbatim, the rubric in its original order,
  the labels unchanged as Post A and Post B, and **the two posts exchanged in presentation
  position**. Nothing else moves. Canonicalisation follows the existing §8 rule, so V4's winner is
  compared against the other variants after mapping back to the canonical skill/baseline sides.
- **Stability, extended:** a pairing is stable when **all four** variants return the identical
  canonical winner and none is unparseable. The audit remains 18 pairings, now 72 calls per seat.
- **The bar does not move.** Pass remains stability >= 90% over the 18 pairings, exactly as
  registered in §4 and unchanged since before any 0.1-L1 datum existed. Widening the test while
  lowering the bar would defeat the purpose.

### 13.3 Re-qualification, and what stands

- **A seat audited under the three-variant definition is not qualified for any cycle run under
  revision d.** §12.1's "an audited seat is never re-audited" continues to bind *within* an audit
  definition; it cannot carry a seat across a change in what the audit tests. Existing
  `_judge-audit-*.json` artifacts stand permanently as records of the test they actually ran, are
  never rewritten, and a re-audit under revision d writes a separate artifact.
- **The 0.1-L1 grades already recorded stand as recorded.** Records are append-only and nothing is
  re-scored after the data has been seen. Report 002 publishes them together with the transcript
  audit that finds them uninterpretable as a measure of craft; that disclosure, not a silent
  revision, is the sanctioned response.
- **Expected consequence, stated in advance:** the currently locked seat is unlikely to survive its
  own corrected exam. That outcome is pre-registered as acceptable and publishable, on the same
  terms as the three refusals that preceded it.

### 13.4 Scope

Revision d changes the qualification gate and nothing else. The 5% unparseable ceiling, the grade
bands, the consistency cap, the head-to-head exclusion (§6.6), the held-out disclosure (§6.8) and
the exhibit rules (§12.4) all carry unchanged. D9's precondition 1 now reads against the
four-variant audit: no tournament datum exists until a seat passes it.

## 14. Revision e — seats 5 and 6, the 30B class (2026-08-30)

**Status: RATIFIED by the owner 2026-08-30**, before either candidate had judged a single pairing,
with the seat order, the stopping rule, the disclosed exception and the falsifiable prediction all
fixed in advance and committed at `d263a91` prior to ratification. Revision d changed the TEST; this changes the SEAT LIST, and the two are deliberately
separate amendments so neither can be tuned to the other's outcome.

### 14.1 What the revision-d re-audit established

All four pre-registered seats failed the four-variant gate on 2026-08-30
(`_judge-audit-d-*.json`): llama3.1:8b 16.7%, gemma2:9b 0.0%, phi4:14b 5.6%,
mistral-nemo:12b 0.0%. The seat that scored 18/18 under three variants scored **0/18** under four,
and all 18 of its unstable pairings flipped on the order swap alone, with the three
prompt-perturbation variants agreeing throughout. That is position dependence with no rubric or
label contamination mixed into it.

**The tested set was narrow in exactly one way that matters: every seat was 8B to 14.7B.** No
conclusion about locally hostable judges in general follows from four models of one size class,
and this amendment tests the next class up rather than declaring the lane dead.

### 14.2 The seats, in pre-registered order

5. **`glm-4.7-flash:q4_K_M`** — Zhipu, `glm4moelite`, 29.9B, Q4_K_M. A house no seat has used,
   disjoint from the generator, and the largest candidate that satisfies §12.1 unmodified.
6. **`gemma4:31b`** — Google, `gemma4`, 31.3B, Q4_K_M. This **repeats a house that already failed**
   (gemma2:9b at 9.2B) at 3.4x the parameter count, which is a deliberate and disclosed relaxation
   of §12.1's no-family-repeats rule among seats. The justification is that a same-house baseline
   is what makes it the single most informative size test available: gemma2 scored 83.3% under
   three variants and 0.0% under four, so any movement at 31.3B is attributable to scale rather
   than to house.

**Generator-disjointness does NOT relax.** `qwen3.6:35b-a3b` (36.0B) and every other qwen3.x build
on the host is excluded despite being the largest model available, because it shares the family of
the pinned generator `qwen2.5:7b-instruct-q4_K_M`. A judge drawn from the generator's house risks
self-preference on the generator's own output, and the research record (`eval-craft-research-v0.1`,
`frontier-self-improvement-research-v0.2`) documents self-preference as the bias frontier judges
show most strongly. Size does not buy an exception to it.

The `-64k` builds already on the host are excluded as duplicates: they are the same weights at a
different context configuration, and `num_ctx` is pinned at 16384 by §3 regardless.

### 14.3 Everything else carries unchanged

The audit is the four-variant gate ratified as revision d: 18 frozen pilot pairings, variants
V1-V4, stability requiring the same canonical winner across all four with none unparseable, and
**the 90% bar, which has not moved since before any 0.1-L1 datum existed and does not move here.**
Digest pinning (§12.3), the append-only artifact rule and the revision-scoped filenames (§13.3)
all carry. Both models are already on the host, so no download is required by this amendment.

### 14.4 Stopping rule, fixed in advance

Seats are audited in the order above and the first to pass locks. **If both fail, the only
further candidate admissible without a new amendment is the conditional seat 7 in §14.7, and no
other seat is improvised.** Once that is exhausted, six or seven seats across five or six houses
and two size classes is the result, and the lane decision it forces (a larger local judge, a paid
frontier judge, or a pre-registered ensemble whose held-out perturbation is fixed before it runs)
is the owner's alone.

### 14.5 The prediction, recorded before the run

Stated so it can be wrong. **Expectation: both seats fail, and the 30B class shows materially
higher stability than the 8-14.7B class without reaching 90%.** The reasoning is that position
bias is documented as a capability gradient rather than a threshold, so scale should move the
number without eliminating the defect. **A result at or near 0% for either seat would contradict
this and would point away from capability and toward the material** — that the frozen audit
pairings may not be separable enough for any judge to discriminate on, in which case position
fills a vacuum rather than overriding a signal. That alternative is not tested by this amendment
and would need its own.

### 14.6 Disclosed exception: `gemma4:31b` does not pin quantization in its tag

§3 requires an explicit quantization in every pinned tag so a tag cannot silently re-point.
`glm-4.7-flash:q4_K_M` satisfies it. **`gemma4:31b` does not**, because it is a locally built tag
on this host rather than a registry tag carrying the quantization in its name. The runtime reports
`Q4_K_M` for it today.

This is recorded as a **named exception, not a relaxation**: the rule continues to bind every other
seat, and the exception is encoded by name in the test rather than by loosening the pattern, so
adding a seventh unpinned seat fails the suite.

**Residual risk, stated rather than waved away.** The digest pin (§3, §12.3) records
`31.3B Q4_K_M` immediately before this seat's first audit call and aborts any later stage on a
change, so the tag cannot drift mid-cycle undetected. What the digest pin cannot protect is the
first resolution itself: if the local tag were re-pointed before that first call, the wrong build
would be pinned without anything noticing. On a single-user host with a locally built model that
risk is low, and it is the reason this is disclosed here rather than left implicit.


### 14.7 Conditional seat 7: DeepSeek, admissible ONLY on an architecture check

DeepSeek is pre-registered as seat 7 **conditionally**, because the family rule cannot be applied
to it by name. Most DeepSeek builds that fit 48 GB are **R1 distills onto another house's base**:
`deepseek-r1:32b` distils onto a Qwen base and `deepseek-r1:8b` onto Llama. A seat named
"deepseek" can therefore BE Qwen, which is the generator's family, and would smuggle past a
name-level check the exact violation §14.2 exists to prevent. The genuinely DeepSeek-architecture
models (V3 / MoE, 671B) do not fit this host at any quantization.

**The admission test, fixed here and verifiable:** after the model is pulled, `ollama show` must
report an `architecture` that matches neither `/qwen/i` nor `/llama/i`. If it matches either, the
candidate is **inadmissible as a seat regardless of its name**, the result is recorded, and no
substitute is improvised in its place. If it passes, it is appended as seat 7 and audited under
the same four-variant gate and the same unmoved 90% bar.

This is pre-registered before any DeepSeek model exists on the host, so the check cannot be
reinterpreted once its architecture is known. Its download, like every other, is surfaced by name,
source and size before it runs.

## 15. Harness defect repair — the reasoning channel (2026-08-30)

**This is a defect repair, not a threshold change.** No pre-registered parameter moves: the 90%
bar, the four-variant gate, the 5% unparseable ceiling, the 512-token judge cap and the §8 schema
rule are all untouched. What changed is that the harness now reads the channel the runtime actually
wrote to.

### 15.1 The defect

`src/safety/local-provider.ts` read only the runtime's `response` field. A **reasoning model**
leaves `response` empty and returns its generated text in `thinking`. Reading `response` alone
therefore recorded a perfectly schema-conformant verdict as `unparseable`, and did so **silently**:
the call succeeded, the one registered retry produced the same empty string, and the seat was
disqualified.

Seat 5 `glm-4.7-flash:q4_K_M` was disqualified this way on 2026-08-30 with **72 of 72 audit calls
unparseable and a recorded stability of 0.0%**. Diagnosis on a real judge pairing, outside the
harness and writing no record: `response` 0 chars, `thinking` 797 chars containing complete
schema-conformant verdict JSON with substantive reasons, `done_reason: stop`, 169 tokens generated
against the 512 cap. **The model complied fully. The harness could not see it.**

`gemma4:31b` was checked the same way and returns its verdict in `response` with no `thinking`
field, so its audit is unaffected and stands.

### 15.2 The repair, deliberately narrow

`generatedText()` returns `response` whenever it has content, so every non-reasoning seat is read
exactly as before and remains comparable to the seats already audited. `thinking` is consulted
**only** when `response` is empty or whitespace, which is precisely the case that was previously
unrecoverable. Where neither channel has content the §8 unparseable rule applies unchanged.

Reading the right channel is not the same as relaxing what counts as a valid verdict: whatever
`generatedText()` returns is still held to the full registered schema. Two regression tests lock
this, including one that reproduces the exact 72/72 shape and asserts the old read still yields
`null`, so the silent-disqualification path cannot return unnoticed.

### 15.3 The invalid artifact is quarantined, not deleted

The seat-5 artifact records what a defective harness produced and must never be read as a
measurement of that model. Following the precedent set for the contaminated 2026-07-31 rater
ratings, it is **renamed rather than deleted**, so the evidence survives and nothing can score it:

    _judge-audit-d-glm-4-7-flash-q4-K-M.HARNESS-DEFECT-2026-08-30.discarded.json

Seat 5 is then re-audited under the repaired harness and writes the canonical artifact name. Its
re-audit is a first measurement of that seat, not a second attempt at one.

### 15.4 The class of failure this closes

Any reasoning model, at any size, from any house, would have scored 0.0% on this harness and
looked like a total capability failure. That is the same shape as the defect revision d fixed: an
instrument that is silent about the thing it cannot see, and a result that looks like data. It is
recorded here so the next reasoning candidate is not disqualified by our own code.

## 16. Revision f — a seat is a model IN A MODE, and every call records how it was made (2026-08-30)

**Status: RATIFIED by the owner 2026-08-30**, before any seat had run under it. The mode split and
the provenance requirements below were fixed before the first (model, mode) seat was audited.

### 16.1 What we did not know we were doing

Until 2026-08-30 the harness **never sent the `think` parameter**. Every seat therefore ran in
whatever mode the runtime chose by default, and no artifact recorded which mode that was. The
results are not wrong, but they are **under-specified**: nothing in the record says how to
reproduce them, which is the definition of an unreplicable measurement.

Measured on the host, same prompt, same sampling, only the mode changed:

| model | `think: true` | `think: false` |
|---|---|---|
| `glm-4.7-flash` | verdict in `thinking`, 15 tokens, terse | verdict in `response`, 60 tokens, fuller reasons |
| `gemma4:31b` | accepted | accepted |
| `llama3.1:8b` | **ERROR, "does not support thinking"** | accepted |

`glm-4.7-flash` in the two modes is not one judge measured twice. It is two judges.

### 16.2 Seat identity becomes (model, mode)

A **seat** is a model tag **and** an explicit thinking mode. `glm-4.7-flash:q4_K_M @ think=false`
and `glm-4.7-flash:q4_K_M @ think=true` are distinct seats, audited separately, with separate
artifacts, and are never averaged or reported as one model's result.

- **`think` is sent explicitly on every call and never left to a runtime default.** A default is a
  hidden variable, and a hidden variable is the thing this amendment exists to remove.
- **`think: false` is the control mode.** It is accepted by every model tested, including those
  with no reasoning capability, so it is the mode in which any two seats are comparable.
- **`think: true` seats exist only where the runtime accepts the flag.** A model that errors with
  "does not support thinking" is recorded as not thinking-capable and contributes one seat, not
  two. That error is a recorded property of the model, never a failed audit.

**Seats 1-4 are not invalidated.** `llama3.1`, `gemma2`, `phi4` and `mistral-nemo` reject or lack
the flag, so their default was equivalent to `think: false` and their recorded results stand as
control-mode measurements. Seat 6 `gemma4:31b` IS thinking-capable, and its 2026-08-30 audit ran
at the runtime default, verified on this host to route to `response` with no `thinking` content.
That artifact is a valid control-mode measurement, but of a **different failure than the others**:
61 of its 72 calls were unparseable because the model DEGENERATES. On a real judge pairing it
opens with substantive, specific reasons, then collapses into a repetition loop of foreign-language
tokens ("dụng dụng dụng ..."), runs into the 512-token cap mid-string, and emits unterminated JSON.
At temperature 0 with repeat_penalty 1.1.

**So the 30B class is still effectively unmeasured for judging ability.** Seat 5 was disqualified
by a harness defect (§15) and seat 6 never got far enough to be judged on its judgment. Three
distinct failure modes are now on record and must not be conflated: **position dependence**
(seats 1-4), **harness incompatibility** (seat 5, invalid, re-run pending), and **degeneration**
(seat 6). Only the first is a statement about a model's judgment.

§16.4 requires the mode be stamped on every artifact written from here on so no future reader has
to reconstruct it.

### 16.3 Do not narrow the field prematurely

The purpose is breadth with provenance, not breadth instead of it. Reasoning models are an
untested class: the four seats that failed revision d were all non-reasoning, and the first
reasoning candidate was disqualified by our own harness (§15) rather than measured. **No
conclusion about local judges should be drawn until at least one reasoning seat has been measured
in both modes**, and this amendment exists so that measurement is reproducible when it happens.

### 16.4 Every artifact carries the configuration that produced it

Each audit artifact records, alongside the existing fields, a `runtime_config` block naming the
exact call shape: `temperature`, `num_predict`, `top_p`, `top_k`, `repeat_penalty`, `num_ctx`,
`format`, and `think`. It also records `output_channels`, a per-seat tally of how many calls
returned via `response` versus `thinking`.

The channel tally is diagnostic, not decorative: a seat that silently changes channel mid-run, or
splits between the two, is a finding about the runtime that would otherwise be invisible, and it
is exactly the signal that would have surfaced the §15 defect on its first call instead of after
72 of them.

### 16.5 What does not change

The 90% bar, the four-variant gate, the 18 frozen pairings, the 5% unparseable ceiling, the
512-token cap, the §8 schema rule, digest pinning and the append-only artifact discipline all
carry verbatim. This amendment adds provenance and splits seat identity. It relaxes nothing.

## 17. Failure evidence — an unparseable call must carry the reason (2026-08-30)

**Diagnostic instrumentation, not a threshold change.** Nothing pre-registered moves. This adds
evidence to a record that previously discarded it.

### 17.1 Why

§8 records a call that fails extraction as `unparseable` and stops there. That single word is
compatible with at least five completely different causes: an empty channel, a truncated response,
a degeneration loop, unconstrained reasoning prose, and a schema-shaped near-miss. **Only the last
is a statement about the model's judgment.** The other four are statements about the instrument.

This project has now hit three instrument defects that presented as model failures:
- §15, a reasoning model recorded 72/72 unparseable because the harness read the wrong channel;
- seat 6, 61/72 unparseable from a degeneration loop that only a manual probe revealed;
- seat 5 in thinking mode, 69/72 unparseable that a manual probe could **not** reproduce, and
  which remains unexplained at the time of writing.

Every one required hand investigation outside the harness, because the artifact recorded no
evidence. That is a defect in what we record, and it is the reason the third is still open.

### 17.2 What is recorded

When a call fails extraction after its one registered retry, the pairing record gains a `failures`
map keyed by variant, holding the runtime `channel`, the full output length in `chars`, and the
first **400 characters** of the raw output as `sample`.

Only failures are captured, so a clean seat's artifact is unchanged in size and shape. 400
characters is chosen as the smallest window that reliably distinguishes the five causes above from
one another: truncation shows a severed string, degeneration shows repetition, reasoning prose
shows narration, an empty channel shows nothing, and a near-miss shows well-formed JSON with the
wrong field types.

### 17.3 What it does not change

The §8 rule is untouched: a response must still parse AND conform to the registered schema, there
is still exactly one retry, and a failure still records as a tie with reason `judge-unparseable`.
Capturing why a call failed is not the same as accepting it. **No salvage parsing is introduced,
and none is permitted:** the sample is evidence for a human reading the record, never an input to
the verdict.

## 18. Revision g — the runtime enforces the registered schema (2026-08-30)

**Status: RATIFIED by the owner 2026-08-30**, before any seat ran under it, with the pre-flight
table in §18.2 measured and committed beforehand.

### 18.1 The harness has been under-implementing §8

§8 requires a judge response to "parse as one JSON object AND conform to the registered schema",
and `src/types.ts` calls the responses "grammar-constrained JSON". The harness passed
`format: 'json'` to the runtime, which guarantees only that the output is **valid JSON** — not that
it matches our schema. A model could satisfy the runtime completely and still fail §8 on a missing
field or a quoted integer, and two seats did exactly that.

Ollama accepts a full JSON Schema in that same field and constrains decoding to it. Passing the
registered schema is therefore not a new rule; it is the rule §8 already states, enforced where it
can actually be enforced rather than only checked after the fact.

### 18.2 Measured, not assumed

`scripts/seat-preflight.mjs` probes each candidate on three real pairings across both thinking
modes and both format settings, writing nothing. Run on 2026-08-30 over all six candidates:

| seat | think | `format:'json'` | `format:schema` |
|---|---|---|---|
| llama3.1:8b, gemma2:9b, phi4:14b, mistral-nemo:12b | false | PASS | PASS |
| the same four | true | not supported by the model | not supported |
| glm-4.7-flash | false | PASS | PASS |
| **glm-4.7-flash** | **true** | **reasons-missing x3** | **PASS** |
| gemma4:31b | false | PASS | PASS |
| **gemma4:31b** | **true** | **margin-string x2** | **PASS** |

Two consequences follow directly, and neither is a judgement call:

1. **Comparability is preserved.** Every control-mode seat conforms identically under both
   settings, so seats 1-6 are unaffected by the change and their recorded results stand.
2. **The two reasoning seats were disqualified by the instrument.** Their 0.0% scores are
   artifacts of an unenforced schema, not measurements of judgment, and both are quarantined and
   re-audited under revision g.

### 18.3 What does not change

Extraction is untouched: §8 still parses the returned text and still checks the full schema, so a
runtime that ignores the constraint is still caught. There is still one retry, a failure still
records as `judge-unparseable`, and **no salvage parsing is introduced**. The 90% bar, the
four-variant gate, the 18 frozen pairings, the 5% ceiling, the 512-token cap and digest pinning all
carry verbatim.

### 18.4 The process change that matters more than the fix

Four instrument defects were found on 2026-08-30, each by a full audit that produced a "0%" which
was not a result: the wrong channel read (§15), a degeneration loop, a missing field, and a quoted
integer. Every one cost a full audit and a manual investigation, and none was a model failing to
judge.

`seat-preflight.mjs` exists so that stops. **No seat is worth a full audit until its cell reads
PASS**, and the screen costs three calls per cell against the real material. Finding four defects
serially is four wasted cycles; finding them in one pass is a pre-flight.

## 19. Correction — the family rule was applied by brand, not architecture (2026-08-30)

**A recorded error in this amendment's own reasoning, disclosed rather than quietly fixed.**

§12.1 states that the seat list "spans five houses: generator Alibaba; seats Meta, Google,
Microsoft, Mistral — no family repeats". That is true by **vendor**. The runtime's reported
architectures say otherwise:

| seat | vendor | `ollama show` architecture |
|---|---|---|
| llama3.1:8b | Meta | `llama` |
| **mistral-nemo:12b** | Mistral | **`llama`** |
| phi4:14b | Microsoft | `phi3` |
| gemma2:9b | Google | `gemma2` |

**Seats 1 and 4 share the `llama` architecture.** The original four seats were therefore less
diverse than pre-registered, and mistral-nemo's 18/18 pure position dependence sitting beside
llama3.1's 10-of-15 may reflect a shared architecture rather than two independent observations.

**Binding correction:** any future disjointness claim is made on the reported **architecture**, not
on the vendor's brand. Existing artifacts stand as recorded; nothing is re-scored. This is a
disclosure about how the evidence should be read, not a change to it.

### 19.1 The generator pin is a choice, not a constraint

§14.2 excludes every qwen build at any size because it shares the pinned generator's family. That
protects 0.1-L1, whose 99 records qwen2.5 generated, from self-preference. It also permanently
excludes the two largest models on the host (35.1B and 36.0B) from ever judging.

**Self-preference is a PAIRING problem, not a property of a model.** A future cycle may pin a
non-qwen generator, which frees those models as judges without weakening the rule. That option was
never put to the owner and is recorded here so it is not lost.

### 19.2 A class of candidate never tried

Every seat audited to date is a general instruction-tuned chat model pressed into judging. Models
fine-tuned specifically for evaluation exist — Prometheus-2, JudgeLM, Auto-J — as do reward models
such as Skywork-Reward and ArmoRM. **None has been tried.** A model trained for fine-grained
evaluation is a better prior than a larger chat model, and one pre-flight cell settles each.
Availability under the pinned runtime must be VERIFIED before any of them is pre-registered.

## 20. Provenance repair — an artifact must record the call that was actually made (2026-08-30)

**Defect repair and instrumentation, not a threshold change**, in the same class as §15 and §17.
Nothing pre-registered moves: the 90% bar, the four-variant gate, the 18 frozen pairings, the 5%
unparseable ceiling, the 512-token cap, the §8 schema rule and digest pinning all carry verbatim.
No recorded number changes and no artifact is rewritten.

### 20.1 Two artifacts describe a call that was never made

§16.4 requires every artifact to carry `runtime_config`, "the exact call shape". The audit writer
spelled that block **by hand**, including the literal `format: 'json'`. Revision g then changed the
call to send the registered schema in that field (§18.1) and the hand-written literal did not move.

Both revision-g artifacts therefore record `"format": "json"` for calls that were made with
`format: <judge_schema>`:

| artifact | audited_at | records | was sent |
|---|---|---|---|
| `_judge-audit-d-gemma4-31b-think.json` | 2026-08-30T16:45Z | `format: "json"` | the registered schema |
| `_judge-audit-d-glm-4-7-flash-q4-K-M-think.json` | 2026-08-30T16:15Z | `format: "json"` | the registered schema |

Both stand as recorded. **Records are append-only and the response to a defect is disclosure, never
revision.** Their stability figures, 22.2% and 16.7%, are unaffected: the constraint that was
actually applied is the stricter one, which is what §18 ratified, and the extraction check that
produced the verdicts is unchanged. What was wrong is the field that claims how they were produced,
which is precisely the field §16.4 exists to make trustworthy. Seats audited before revision g
recorded `format: 'json'` and were sent `format: 'json'`; those artifacts are accurate.

### 20.2 The repair is a control, not a reminder

`requestFormat()` in `local-provider.ts` is now the single expression producing the value, and
`localGenerate()` puts on the wire exactly what it returns. `JUDGE_CALL_SHAPE` in `local-cycle.ts`
is the single description of a judge call: the audit call spreads it and `judgeRuntimeConfig()`
projects it into the artifact. Neither can drift from the other without the other moving too.

A provenance field written by hand beside the call it describes will drift. That is not a lesson to
remember, it is a defect to design out, and this is the fourth time in this project a rule that had
to be remembered was not.

### 20.3 The mandatory screen was not screening what the audit runs

`scripts/seat-preflight.mjs` is mandatory before any seat audit
(`DEC-preflight-before-any-seat-audit`), so a PASS cell is the evidence on which a full audit is
spent. Its schema was a **hand-copied literal** that had drifted from `PROTOCOL_L1.judge_schema` in
two independent directions at once:

- it added `enum: ['A','B','tie']` to `winner`, which the registered schema does **not** have, so
  the screen **constrained decoding more tightly** than the audit does; and
- its conformance check tested only `typeof winner === 'string'` and ignored the margin range,
  while `extractJudgeVerdict()` rejects a winner that is not one of the presented labels or `tie`
  and rejects a margin outside 1..3, so the screen **accepted verdicts the audit calls
  unparseable**.

A PASS cell was therefore weaker evidence than it read, in both directions. The screen now derives
the schema from `src/types.ts`, the way it already derived the judge header from `src/judge.ts`, and
rejects both cases the audit rejects. `checks/preflight_fidelity.mjs` runs inside `npm run verify`
and holds it there; the gate was proven against a deliberately reintroduced enum before it was
wired in.

### 20.4 What this does not change

No verdict, stability figure, grade or interval moves. §18.2's pre-flight table stands as recorded
of the test it actually ran, and is read with §20.3 beside it. The two artifacts named in §20.1 are
never rewritten. Any artifact written from here on records the literal value the runtime received.

## 21. The qualification gate binds downstream, not only at the audit (2026-08-31)

**Defect repair, same class as §15, §17 and §20.** Nothing pre-registered moves: the 90% bar, the
four-variant gate, the 18 frozen pairings, the 5% ceiling, the 512-token cap, the §8 schema rule and
digest pinning all carry verbatim. No recorded number changes and no artifact is rewritten. This
implements a rule §13.3 already states; it does not add one.

### 21.1 The rule was written and nothing enforced it

§13.3 says a seat audited under the three-variant definition "is not qualified for any cycle run
under revision d", and `auditStage` accordingly writes its lock to
`_judge-seat-${AUDIT_REVISION}.json`. **Every consumer read the unversioned `_judge-seat.json`:**

| call site | what it decides |
|---|---|
| `judgeStage` | which model judges every pairing of a cycle |
| `scoreStage` | the seat named in the cycle manifest |
| `pinModelDigests` | which model's digest is pinned against mid-cycle drift |
| `tournament.pinActiveDigests` | **D9 precondition 1**, "no tournament datum exists until a seat passes it" |

That file still exists and still names `mistral-nemo:12b-instruct-2407-q4_K_M`, which passed the
three-variant audit at 18/18 and then scored **0.0%** under the four-variant gate that binds, with
all 18 unstable pairings flipping on the order swap alone. `npm run local-cycle -- --local judge`
would have judged a cycle with it, and the tournament would have passed its own precondition on it.

**This is the same shape as every other defect on record here: a rule that has to be remembered is
not a control.** The amendment stated the rule, the audit honoured it, and four consumers did not.

### 21.2 One accessor, and it refuses by name

`lockedSeat(root)` is now the only way a stage learns which seat may judge. It reads the
revision-scoped lock and returns a `{model, think}` pair, because §16 makes a seat a model **in a
mode**. A lock written before revision f carries no mode and reads as `think: false`, which is what
those models measured, so `think` never reaches the runtime as `undefined`.

A superseded lock is **refused by name**, not ignored: a stage that reports "no seat is locked"
while a lock file plainly sits on disk sends the next reader looking in the wrong place. The
refusal names the stale seat, the binding revision, and the fact that the artifact stands as a
record of the test it actually ran.

`lockedSeatOrNull()` serves the scorer, which must be able to write a manifest for an ungraded
cycle. It returns `null` rather than a superseded seat, so a manifest records `unlocked` rather
than a name that would read as qualification.

### 21.3 Two further defects found in the same call sites

- **`judgeStage` never sent `think`.** §16.2 requires the mode explicitly on every call, never left
  to a runtime default. Every judged pairing of the graded 0.1-L1 cycle therefore ran in an
  unrecorded mode. The mode now comes from the lock.
- **The tournament's judge call sent `jsonFormat: true` with no schema**, so it carried the exact
  §18 defect revision g repaired, and restated the call shape by hand where it could drift from the
  audit's. Both stages now spread `JUDGE_CALL_SHAPE` (§20), so a seat is judged with the shape it
  was qualified with.

### 21.4 The control

`checks/seat_lock_gate.mjs` runs inside `npm run verify` and fails if any file under `src/` outside
the accessor names a seat lock directly, or if the writer and the readers stop deriving the path
from the same `AUDIT_REVISION`. It was proven RED against a deliberately reintroduced direct read
before being wired in, and the refusal itself was proven against the record host's live state: the
judge stage stopped with the named error and the 324 committed judgments were untouched.

### 21.5 What this does not change

No verdict, stability figure, grade or interval moves. The 0.1-L1 grades stand as recorded with
Report 002's disclosure. `_judge-seat.json` is **not deleted**: §13.3 makes it a permanent record of
the test it ran. No seat is qualified under revision d, so the practical effect is that judging,
scoring against a seat, and the tournament all now stop where the protocol always said they should.
