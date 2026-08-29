# Amendment D9 — The Tournament Lane (0.1-T1): scan, challenge, forge, repeat

**Date:** 2026-08-28 (revision a, pre-review) · 2026-08-29 (revision b — incorporating the
16-finding adversarial review, §9, and the owner's three Layer-4/5 decisions, recorded verbatim
in §0).
· **Status:** DRAFT for owner ratification; everything below is **pre-registered before any
0.1-T1 datum exists** and binds at the commit gate when the owner approves the ratification
diff. This is the pre-registration that D8 §6.6 names as its own unlock ("A future head-to-head
cycle needs its own pre-registration").
· **Scope:** protocol lineage + matrix shape + scoring + the scan/forge stages of the loop.
· **Serves:** `PREMISE.md` — the five-verb loop. This amendment is the CHALLENGE verb's
instrument and the SCAN verb's entry rules; §5 binds the BUILD verb to ratified doctrine.

## 0. The owner's three decisions (2026-08-29, binding on this revision)

1. **Bracket winner = beat every opponent, never a pool average** — "our goal is a fact based
   theoretical best." §3 registers the per-opponent rule.
2. **When the data cannot separate, scale the matrix until separation is realistic** — a
   leaderless cycle is a waypoint with a registered escalation, never a terminal state. §3.5.
3. **The human leaves the loop.** "The agents find a winner and have a skill built in my name
   that beats all others — that's the end state," extended to every marketing/AI-marketing
   JTBD. The owner uses skills in real life when he needs them and comments then; the loop
   never waits for his review. His feedback becomes a recorded signal (§5.4), his rank is no
   promotion gate anywhere in this lane, and the calibration tier survives only as what
   unlocks crowning LANGUAGE (§6.1) plus periodic judge-validity checks — a deliberate,
   disclosed rescoping of the human tier for this lane (§8).

## Problem being amended

The premise's challenge verb has no runnable instrument: head-to-head pairing generation exists
in code (`src/judge.ts`) and ran in the 2026-07-18 pilot, but the only executable lineage
(0.1-L1) excludes skill-vs-skill judging and forbids any skill-vs-skill claim (D8 §6.6). No
starting champion can be named from existing data because the v0.1 brief set measured 0%
separability (`DEC-separability-before-ranking`) and S1–S3 are not held-out for the nine own
skills (D8 §6.8). The tournament therefore needs: its own version string, fresh
separability-gated briefs, a per-opponent scorer, brackets, scan-stage entry rules, and
pre-registered claim wording — all fixed before data.

## 1. Version, lineage, and preconditions

- **Protocol version string: `0.1-T1`** (tournament, local lane). A sibling of 0.1-L1 sharing
  its execution plane verbatim — loopback-only runtime, pinned generator
  `qwen2.5:7b-instruct-q4_K_M`, pinned sampling (D8 §3), digest discipline (D8 §12.3),
  judge-call rules (D8 §8), E27 governed writer, append-only version-scoped subtrees
  `runs/0.1-T1/`, `judgments/0.1-T1/`, `scores/0.1-T1/`. The flat v0.1 trees, the 0.1-L1
  trees, and briefs S1–S3/S4–S9 are never touched.
- **Skill-vs-skill claims become possible ONLY for 0.1-T1 records**, in the §6 wording. D8
  §6.6's ban continues to bind every 0.1-L1 record forever.
- **Preconditions, hard and ordered — no tournament datum before all four:**
  1. A judge seat locked by the D8 §4/§12.1 perturbation audit (no seat, no bracket; four
     refusals → the owner's lane decision comes first).
  2. The 0.1-L1 baseline cycle completed through score and Report 002.
  3. T-series briefs authored and passed through the §4 brief gate.
  4. Owner ratification of this amendment plus the per-item approvals it names (downloads,
     every corpus ingest, every cycle's bracket addendum).

## 2. The scan stage (T-scan): how a skill enters the bracket

- **Discovery runs OUTSIDE the offline harness and never changes that.** Session agents
  research popular marketing-skill repositories with ordinary web access and produce a
  **candidate register** per candidate: source URL, acquisition method, license evidence (file
  or its absence), the skill's OWN claimed job quoted verbatim, estimated tokens. The harness
  gains no network path; M0/D8 §9 carry unchanged.
- **Entry is a governed, append-only ingest** (manifest per the data-schema/D8 §5 shape):
  content hash over the Declared Load Set, provenance, species, license class, `jtbd_claim`
  as claimed by the skill's own text or marketing with the claiming quote recorded. The owner
  approves every ingest per item. **Machinery to build, disclosed:** the current external
  ingest (`src/ingest.ts`) is a frozen M0 read-only preview; a governed external-ingest path
  (the `ingest-new` pattern extended beyond `hendry-*`) is part of this amendment's
  implementation and lands with it, tested, before any T-scan ingest runs.
- **Pseudonymous-at-birth for externals (the anonymization fix):** every newly ingested
  external skill receives a registry pseudonym slug (`ext-a`, `ext-b`, …, assigned in ingest
  order) used in ALL record ids, manifests, and arm keys; the pseudonym↔identity mapping
  (name, URL, claiming quote) is committed in this private repository in
  `benchmark/corpus/manifests/_external-identities.json` and **never mirrors** (it joins the
  mirror allowlist's refuse list). Publication then follows §6.3 without rewriting append-only
  records — the precedent is the pilot's `proprietary-incumbent-1`
  (`DEC-anonymize-incumbent-grades`), which showed anonymization must happen BEFORE records
  exist. **Grandfathered pilot manifests** (`corey-social`, `eric-x-longform-post`,
  `diandra-linkedin-hook-writer`) stand as-is under their existing slugs (D8 froze them);
  each may enter a bracket only after a §2-conformant addendum manifest (claiming quote,
  license class) is committed, and their real-slug record ids carry the §6.3 residual-linkage
  disclosure.
- **License classes, pre-registered:** `permissive-with-notice` (raw text ingested with the
  upstream LICENSE bundled; measurable; text quotable per license; named grades still gated by
  §6.3/protocol §11); `proprietary-author-released` (private measurement only; no text
  publication, no named grade without consent); `no-license-found` (treated as proprietary in
  every respect). **No skill's text ever feeds a builder regardless of class** (§5.1).
- **Off-claim runs stay findings, never grades** (the Q1 rule carries verbatim): a skill
  judged outside its claimed JTBD appears only in "off-category — exhibited, not graded",
  never pooled, never ranked. **The manifest's `jtbd_claim` is the sole authority for bracket
  eligibility.**

## 3. The challenge stage (T-challenge): the bracket

1. **One claimed JTBD per bracket, one bracket per cycle.** Every cycle's arms are enumerated
   **by slug + content hash** in a cycle addendum committed BEFORE any generation; the bracket
   never changes after data exists.
2. **First bracket, enumerated now: `li-full-post`, six skill arms** — `corey-social`
   (`214bb4236c67eb1996fdff5884bcb30b97b0b784f51e3b57163f4ef03f1fd13d`, on-claim
   `li-full-post`, MIT, grandfathered per §2) and the five on-category hendry skills per their
   D8 §5 hashes. `eric-x-longform-post` claims `x-longform` and `diandra-linkedin-hook-writer`
   claims `li-hook`: **both sit out** per §2's off-claim rule and wait for their own brackets
   (the revision-a draft seated eric off-claim; the review caught it — §9 F1). Plus B0 and B1:
   eight arms.
3. **Matrix (first bracket):** **five T-briefs (T1–T5) × 3 runs per arm**, generator and
   sampling verbatim from D8. Five briefs, not three, because the §3.6 naming gate needs
   brief-clustered uncertainty and three clusters is a degenerate bootstrap (§9 F4). Pairings:
   every skill vs B0 and vs B1, plus round-robin skill-vs-skill, run-matched within each
   brief, both presentation orders. Volume: baselines 6×2×15 = 180 pairings → 360 calls;
   head-to-head C(6,2)=15 pairs × 15 = 225 pairings → 450 calls; **≤ 810 judge calls**,
   an estimated 13–27 hours on the mini at 1–2 min/call.
4. **Judge:** the D8-locked seat; judge prompt verbatim (protocol §6 — the pilot ran
   head-to-head with it unchanged); temp 0; `format: json`; D8 §8 parse/retry/tie rules and
   the 5% unparseable validity floor over all tournament calls. Gate semantics carry verbatim
   from v0.1 as implemented, including head-to-head gate handling exactly as the pilot
   recorded it. **Gate-decided share is reported per arm and per opponent-pair**, and any
   pair whose verdicts are majority gate-decided is flagged in the report (a gate, not craft,
   separated them).
5. **Scoring — per-opponent, never pooled (owner decision 1):** for each ordered skill pair
   (A, opponent), the statistic is A's head-to-head WR against that opponent (win 1 / tie
   0.5 / loss 0 over the pair's judged pairings), with a **brief-clustered seeded bootstrap
   95% CI** (resample the five briefs with replacement, seed 42, 10,000 resamples — the
   resampling unit is the brief, because pairings within a brief share generations and are
   not independent; §9 F4). WR_B0 and WR_B1 per skill carry the same clustered CI. Order-swap
   consistency is reported per pair. A pooled average is computed for descriptive context
   only and **decides nothing**.
6. **Leader rule (registered, three conditions, all required):** the bracket leader is the
   arm that (a) **beats every other skill arm**: per-opponent WR CI lower bound > 0.5 against
   EACH opponent; (b) **clears the floor as a registered condition, not rhetoric** (§9 F6):
   WR_B0 CI lower bound > 0.5; (c) **the bracket separates**: benchmark separability for the
   T-brief set (the committed instrument `scripts/scout/separability.mjs`, Arena-Hard
   pairwise-CI-disjointness) is computed BEFORE any ranking is read, per
   `DEC-separability-before-ranking`, and a bracket at ~0 separability reports its ranking
   as **uninformative — no leader, regardless of any single arm's CIs** (§9 F2). If no arm
   meets (a)–(c): **"no leader named this cycle"**, and §3.7 fires. At most one arm can
   satisfy (a); no tie-break is needed or registered.
7. **The escalation ladder (owner decision 2 — scale until separation is realistic):** a
   leaderless or unseparated cycle triggers, in registered order, each step a completion
   (existing records stand, never re-run), each committed as an addendum before its data:
   **E1** +3 runs per arm on the same briefs (draws r4–r6; +810 calls);
   **E2** +2 fresh T-briefs through the §4 gate (T6–T7; matrix extends to them);
   **E3** the owner's lane decision (a bigger local judge per D8 §12.1's list, or the paid
   v0.2 panel). The ladder is walked at most one step per cycle; no other extension of a
   bracket's data is permitted (no optional stopping; §9 F5).

## 4. T-series briefs: fresh, held-out, gated

- **Five briefs (T1–T5)** for the bracket JTBD, same schema as S1–S3, fixture facts fictional
  and labeled. S1–S3 are burned for the nine own skills (D8 §6.8); **S4–S9 are untouchable**
  (`docs/PREREGISTRATION-S4-S9-v3.md`).
- **Authorship blindness, recorded:** T-briefs are authored by a session agent that has not
  read any bracket skill's text in that session; the contamination statement is committed
  with the briefs. Every bracket skill predates the T-briefs, so they are genuinely held-out
  for ALL arms — own and external alike — which S1–S3 never were. Reports state this.
- **Brief gate before any generation:** an independent session-agent panel (findings
  committed) checks realism, answerable constraints, no vocabulary lifted from any bracket
  skill, no fixture-fact collisions, and that the gate battery is computable on each brief's
  constraints. A failed brief is rewritten before the bracket addendum commits, never after.
- **Burn discipline:** once judged in a bracket, T-briefs are burned for forge iteration and
  builder exposure. Forge promotion (§5) always uses fresh P-series briefs authored after the
  variant is frozen. E2 escalation briefs pass the same gate and burn the same way.

## 5. The forge stage (T-forge): the variant that must beat the leader

1. **Clean-room for all classes:** the builder receives the mechanism analyses, this lane's
   judge reasons, its own losing outputs, and aggregate metrics — **never any skill's text,
   permissive or proprietary alike** (stricter than skill-spec v0.2 C2 and simpler to audit;
   `DEC-diandra-proprietary-private-only` generalized; §9 F8). Start-from-winner
   (`DEC-skill-build-start-from-winner`) proceeds on mechanism, not text. The >15%
   similarity gate blocks promotion. Lineage recorded (`LineageEdge`,
   `challenger-iteration`).
2. **The promotion matrix, registered now (§9 F5):** the frozen variant vs the bracket leader
   only, on **five fresh P-briefs × 3 run-matched draws = 15 pairings × 2 orders = 30
   judgments**, same judge, same rules, plus the variant vs B0 (15 pairings × 2) as the floor.
   **The promotion claim** — "beats the leader" — requires the variant's per-opponent WR CI
   lower bound (brief-clustered, as §3.5) > 0.5 against the leader AND WR_B0 CI lower bound
   > 0.5. **One shot per forge cycle:** if the CI does not clear, the claim is "not
   established" and the next forge cycle authors NEW P-briefs — the old promotion set is
   never extended after its data exists (no optional stopping).
3. **Truth and integrity conditions carried, adapted and disclosed (§9 F7):** zero
   `fact_containment` violations in the variant's judged P-runs (the review-severity gate is
   promotion-blocking here), the similarity audit, and the compute-matched-baseline caveat
   (v0.3 Δ15) on any published forge gain. The full Δ16 apparatus (A11 claim pipeline, A9
   negative briefs) belongs to the paid lane and is NOT claimed here; its absence is stated
   wherever a promotion publishes.
4. **The loop after promotion (owner decision 3 — no human gate):** the promoted variant
   enters the next bracket cycle automatically; brackets for further JTBDs are added by cycle
   addenda toward the end state (a hendry variant for every marketing/AI-marketing JTBD).
   **The owner's real-use feedback is a recorded signal, never a gate:** when he uses a skill
   and comments, the comment is committed as a feedback record referencing the skill hash,
   and the next forge cycle for that JTBD must cite it in the builder's inputs. Shipping to
   his own stack remains his private-registry act (protocol §10) whenever he chooses; the
   loop does not wait for it.

## 6. Claims, naming, and publication (pre-registered wording)

1. **The scoped outcome claim** is the only outcome form: "As of <date>, under 0.1-T1 and
   judge manifest <hash>, <skill> beat every tested competitor for <JTBD> on held-out briefs —
   CIs and run data attached." The words "best", "champion", and "crowned" appear in NO
   public claim while the human calibration tier is unqualified (D8 §6.4 carries; "bracket
   leader" is the public term; "champion" only as the internal registry term). The
   calibration tier gates language only — it gates no bracket, no promotion, no cycle (§0.3).
2. **The process claim, always available:** "the loop continuously re-tests these skills
   against the field, and no skill keeps its place after losing."
3. **Protocol §11 governs naming.** New externals are pseudonymous from birth (§2), so
   publication without consent shows the pseudonym + Declared-Load-Set hash and no identity;
   with consent, the mapping entry publishes. Grandfathered real-slug arms (corey) publish
   anonymized at the artifact layer with this **disclosed residual linkage**: their record
   ids in the private repository embed the slug and cannot be rewritten (append-only), so
   anonymization is complete only in published artifacts and the mirror, not against a
   reader of the private repo — stated wherever it applies (§9 F3). Weak external results
   publish as unnamed aggregates. The own nine publish named regardless of result (D8 §6.9).
4. **0.1-T1 numbers never co-quote with 0.1-L1 or v0.1 numbers.** No grade letters exist in
   this lane; brackets report per-opponent WRs and CIs only.
5. **Exhibits:** emitted only by the deterministic generator under a pre-registered T-rule —
   the leader's first real-judge-call pairing vs each opponent it is claimed to beat
   (skill entry leading), and vs B0 — selection never moves after results are seen.
6. **Single-small-judge limitation disclosed wherever 0.1-T1 results appear** (D8 §4 wording
   carries).

## 7. Records, mirror, and safety

- E27 writer, append-only, completion-never-rerun, the one-record crash window disclosed —
  verbatim from D8 §7, applied to the 0.1-T1 subtrees; digests per D8 §12.3.
- **M0 delta, stated narrowly for ratification (the D8 §9 pattern):** the `--local` mode's
  filesystem column gains the three 0.1-T1 subtrees and (append-only)
  `benchmark/corpus/manifests/`. Nothing else moves; no socket rule changes; Phase 1B stays
  locked.
- **Mirror:** 0.1-T1 record classes join `scripts/mirror-allowlist.json` as `proposed` only;
  `_external-identities.json` joins the refuse list permanently. Each class flips to `sync`
  by per-item owner decision after §11 screening; external texts never mirror; outputs
  generated under external-skill instruction carry the named-grade screen before any flip.
- Transcript audit before any report quotes a tournament number: D8 §6.7 verbatim, plus every
  pairing where the two orders disagreed, plus every majority-gate-decided pair.
- **Machinery to build before the first bracket, disclosed as absent today (§9 F9):** the
  external-ingest path (§2), the per-opponent scorer + separability integration
  (`src/`), bracket support in the cycle CLI (arms beyond `hendry-*`; version-scoped 0.1-T1
  subtrees in the runner), the feedback-record class (§5.4), and the T-exhibit rule in the
  exhibit generator. Each lands tested, revision-c style, before the bracket addendum.

## 8. What this amendment does not do

It does not run anything (ratification gates every datum); does not touch v0.1, 0.1-L1,
S1–S9 briefs, or any frozen record; does not ratify A15; does not unlock Phase 1B or any
network path; does not retire the v0.2 rigor layer, which remains the paid lane's bar and
the only path to crowning language; does not put the owner's rank in any gate (his
qualification re-take now unlocks language only); and does not let "always the best" into
any public sentence — the honest forms are §6.1 and §6.2, exactly as `PREMISE.md` records
them. The human-tier prohibition R9 (never retire the κ layer) is honored in its own scope —
the paid-lane panel calibration — while this local lane's human role is rescoped by the
owner's explicit §0.3 decision, disclosed here.

## 9. Cross-agent review disposition (2026-08-29)

Revision a went through a 21-agent adversarial review (5 lenses × find → 66 findings → 16
most-severe skeptic-verified: 14 CONFIRMED, 2 CONFIRMED-with-correction, 0 refuted; the 50
unreviewed minor findings are journaled in the lab session record, workflow
`wf_ba7a999e-f36`). Every confirmed finding is incorporated:

- **F1 eric off-claim in the bracket** (4 duplicate confirmations) → §3.2: eric out, six
  arms, matrix re-registered.
- **F2 leader naming ignored separability** → §3.6(c): separability is a registered naming
  precondition with the uninformative rule.
- **F3 "name withheld" impossible with slugged record ids** → §2 pseudonymous-at-birth for
  new externals; §6.3 residual-linkage disclosure for grandfathered slugs.
- **F4 bootstrap resampled non-independent pairings** → §3.5 brief-clustered bootstrap; five
  briefs so the cluster bootstrap is non-degenerate.
- **F5 promotion matrix unregistered / optional stopping** → §5.2 registered matrix, one
  shot, new P-briefs per forge cycle; §3.7 registered escalation ladder replaces ad-hoc
  scaling.
- **F6 B0 floor rhetorical** → §3.6(b) registered condition.
- **F7 forge dropped Δ16 truth/no-harm** → §5.3 adapted conditions + explicit disclosure of
  what is NOT claimed.
- **F8 forge path unregistered for proprietary leaders** → §5.1 clean-room for all classes.
- **F9 external hashes unpinned / machinery assumed** → §3.2 corey hash pinned +
  grandfathering rule; §2 and §7 disclose every absent piece of machinery.
- Plus the owner's three §0 decisions, which resolve the review's open design questions
  (pool-vs-per-opponent, leaderless handling, the human's role).
