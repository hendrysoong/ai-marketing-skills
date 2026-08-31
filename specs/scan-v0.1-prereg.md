# Pre-registration — Scan, the discovery verb (v0.1)

**Status: DECIDED BY THE AGENT 2026-08-31 under the owner's standing grant of autonomy, and
written BEFORE the first search.** Admission rules fixed here cannot be adjusted to fit what a
sweep happens to turn up. That ordering is the only thing separating a discovery sweep from
cherry-picking.

## 0. Why this verb, and why now

`PREMISE.md` names five verbs. Scan is verb one and it is the **only one blocked on nothing**:
Challenge is gated on a judge that does not exist, and Build, Use and Repeat all consume what Scan
produces. The corpus holds **three external competitors**, all entered by hand in July 2026. A
qualified judge arriving tomorrow would have almost nothing to rank.

## 1. The network boundary, and why Scan is two stages

Amendment D8 §9 and M0 make this repository **offline**: the only sanctioned socket is loopback to
the local runtime, and `checks/static_network_check.mjs` fails the build if egress appears in
application code. Discovery needs the open web. **That contradiction is resolved by splitting the
verb, not by weakening the guarantee.**

- **Stage 1, DISCOVERY (networked, outside the repository).** The agent searches, reads candidate
  repositories, and writes a candidate record per finding. No repository code performs egress and
  none is added. The evidence of a sweep is the ledger it writes, not a socket it opened.
- **Stage 2, INGEST (offline, in-repository).** `ingest-external` consumes a local checkout,
  pins a content hash, captures the license, and writes a corpus manifest. Unchanged by this.

Stage 1 hands Stage 2 a candidate list. Nothing else crosses the boundary.

## 2. Admission rules, fixed before the first query

A candidate is **admissible** only if all of the following hold. Anything else is **rejected with
its reason recorded**, never dropped silently.

1. **It is a skill, not an article.** A single always-on instruction file (`SKILL.md` or an
   equivalent the author designates), loadable verbatim before the model does the work.
2. **It targets a marketing job.** The claimed JTBD maps to the corpus taxonomy, which is open:
   a job with no existing category is admitted and the category recorded, since the benchmark's
   remit is every marketing job and not writing alone.
3. **Its license is resolvable.** Either an SPDX-identifiable license file, or an explicit
   author-released grant. **License class is recorded on every candidate, admitted or not.**
   An unlicensed public repository is admitted for MEASUREMENT with `license: none-stated`, and
   §11 then governs what may be published about it.
4. **It is pinnable.** A commit SHA, release tag, or uploaded archive hash exists, so the measured
   artifact is the artifact named.
5. **It is not already in the corpus**, by content hash OR by `source_url`. Duplicates are
   recorded as `rejected: duplicate` with the slug they duplicate.

**Explicitly NOT an admission criterion: quality, popularity, star count, or how well the skill
reads.** Screening candidates on apparent quality would pre-select the field the benchmark exists
to measure, which is the exact failure the benchmark accuses the market of.

## 3. The sweep is a record, not a browsing session

Every sweep writes to `benchmark/corpus/scan-ledger.json`, append-only:

- the **queries** issued and the date, so a later reader can re-run the sweep;
- every **candidate** found, with `source_url`, `license`, claimed JTBD, and the admit or reject
  decision with its reason;
- candidates that were **found and rejected**, which is the half a browsing session throws away
  and the half that proves the sweep was not curated.

`checks/scan_ledger_gate.mjs` fails the build if a corpus manifest exists with no ledger entry
behind it. The three July 2026 entries predate this and are **grandfathered by name**, disclosed
rather than back-filled with a sweep that never happened.

## 4. Consent, unchanged

Protocol §11 governs **publication, not ingestion**. A skill may be ingested and measured without
contacting the author. Publishing a **named** grade requires consent; weak results publish as
aggregate statistics without names. Scan changes none of this and records `provenance` on every
candidate so the publication decision has what it needs later.

## 5. What this pre-registration does NOT authorise

- No download of anything not named in a ledger entry first.
- No execution of discovered code. Skills are read as text and measured as text.
- No ingest of a skill whose license forbids redistribution into a public corpus without the
  owner's per-item approval.
- No change to the judge, the gate, the briefs, or any 0.1-L1 record.
